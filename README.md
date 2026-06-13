import { useState, useEffect, useRef } from "react";

const COLORS = {
  saffron: "#FF6B00",
  saffronLight: "#FF8C38",
  saffronDark: "#D45500",
  ivory: "#FAF7F2",
  ivoryDark: "#F0EBE2",
  indigo: "#3D2B8E",
  indigoDark: "#2A1D65",
  indigoLight: "#5B45B0",
  gold: "#C9A84C",
  goldLight: "#E8C96A",
  white: "#FFFFFF",
  textDark: "#1A1A2E",
  textMid: "#4A4A6A",
  textLight: "#8888AA",
  success: "#2ECC71",
  cardBg: "#FFFFFF",
  border: "#EDE8F5",
};

const mandalaStyle = {
  position: "absolute",
  opacity: 0.08,
  pointerEvents: "none",
};

function MandalaCircle({ size = 300, color = "#FFFFFF", style = {} }) {
  return (
    <svg width={size} height={size} viewBox="0 0 300 300" style={{ ...mandalaStyle, ...style }}>
      <g transform="translate(150,150)">
        {[0, 30, 60, 90, 120, 150, 180, 210, 240, 270, 300, 330].map((angle) => (
          <g key={angle} transform={`rotate(${angle})`}>
            <ellipse rx="12" ry="60" fill={color} opacity="0.6" />
            <circle cx="0" cy="-75" r="6" fill={color} />
            <ellipse cx="0" cy="-90" rx="4" ry="12" fill={color} opacity="0.4" />
          </g>
        ))}
        {[0, 45, 90, 135, 180, 225, 270, 315].map((angle) => (
          <g key={`inner-${angle}`} transform={`rotate(${angle})`}>
            <ellipse rx="6" ry="35" fill={color} opacity="0.5" />
          </g>
        ))}
        <circle r="20" fill="none" stroke={color} strokeWidth="2" />
        <circle r="40" fill="none" stroke={color} strokeWidth="1" opacity="0.5" />
        <circle r="60" fill="none" stroke={color} strokeWidth="1" opacity="0.3" />
        <circle r="100" fill="none" stroke={color} strokeWidth="1" opacity="0.2" />
      </g>
    </svg>
  );
}

function PaisleyAccent({ color = COLORS.gold, size = 40, style = {} }) {
  return (
    <svg width={size} height={size * 1.5} viewBox="0 0 40 60" style={{ ...mandalaStyle, opacity: 0.15, ...style }}>
      <path d="M20 55 C5 40 2 25 10 15 C18 5 30 8 32 20 C34 32 22 38 20 30 C18 22 26 20 24 28" fill="none" stroke={color} strokeWidth="2.5" />
      <circle cx="20" cy="10" r="3" fill={color} />
    </svg>
  );
}

const categories = [
  { icon: "🔧", label: "Home Repair", labelHi: "मरम्मत", color: "#FF6B00" },
  { icon: "📚", label: "Tutoring", labelHi: "ट्यूशन", color: "#3D2B8E" },
  { icon: "🧹", label: "Cleaning", labelHi: "सफाई", color: "#2ECC71" },
  { icon: "📷", label: "Photography", labelHi: "फोटोग्राफी", color: "#E91E8C" },
  { icon: "💒", label: "Wedding", labelHi: "शादी", color: "#C9A84C" },
  { icon: "💄", label: "Beauty", labelHi: "ब्यूटी", color: "#FF4081" },
  { icon: "💪", label: "Fitness", labelHi: "फिटनेस", color: "#00BCD4" },
  { icon: "⭐", label: "Others", labelHi: "अन्य", color: "#9C27B0" },
];

const professionals = [
  { name: "Rajesh Kumar", job: "Plumber", rating: 4.8, jobs: 142, price: "₹299", city: "Jaipur", verified: true, initials: "RK", color: "#FF6B00" },
  { name: "Priya Sharma", job: "Beautician", rating: 4.9, jobs: 89, price: "₹499", city: "Nagpur", verified: true, initials: "PS", color: "#E91E8C" },
  { name: "Vikram Singh", job: "Electrician", rating: 4.7, jobs: 203, price: "₹349", city: "Lucknow", verified: true, initials: "VS", color: "#3D2B8E" },
  { name: "Anita Patel", job: "Home Tutor", rating: 5.0, jobs: 56, price: "₹599", city: "Surat", verified: true, initials: "AP", color: "#2ECC71" },
];

const quotes = [
  { name: "Suresh Mehta", job: "Plumber", rating: 4.6, price: "₹850", msg: "I can fix this today, have all required tools and 8 years experience.", initials: "SM", color: "#FF6B00" },
  { name: "Ramesh Yadav", job: "Plumber", rating: 4.8, price: "₹1100", msg: "Professional service with 1 month warranty on all repair work.", initials: "RY", color: "#3D2B8E" },
  { name: "Kavita Singh", job: "Plumber", rating: 4.5, price: "₹750", msg: "Quick turnaround, same day service available, free inspection.", initials: "KS", color: "#C9A84C" },
];

const chatMessages = [
  { from: "pro", text: "Namaste! Main aapka kaam aaj 3 baje aa ke dekh sakta hoon. Kya chalega?", time: "10:02 AM" },
  { from: "user", text: "Haan bilkul, 3 baje theek hai. Ghar ka address share kar deta hoon.", time: "10:04 AM" },
  { from: "pro", text: "Perfect! Please location pin share karein. Koi tools laana ho toh bata dena.", time: "10:05 AM" },
  { from: "user", text: "📍 Location shared. Pipe leak hai bathroom mein, please tools laana.", time: "10:07 AM" },
  { from: "pro", text: "Received! Main 3 baje pohunch jaata hoon. Dhanyawad 🙏", time: "10:08 AM" },
];

const jobRequests = [
  { title: "Bathroom Pipe Leak Fix", cat: "🔧", budget: "₹500–₹1000", location: "Vaishali Nagar, Jaipur", time: "2 hrs ago", bids: 3 },
  { title: "Class 10 Maths Tutor", cat: "📚", budget: "₹800/month", location: "Civil Lines, Lucknow", time: "45 min ago", bids: 7 },
  { title: "House Deep Cleaning", cat: "🧹", budget: "₹1500–₹2000", location: "Adajan, Surat", time: "1 hr ago", bids: 5 },
  { title: "Wedding Photography", cat: "📷", budget: "₹15,000–₹25,000", location: "Sadar, Nagpur", time: "3 hrs ago", bids: 12 },
];

// ─── SCREENS ────────────────────────────────────────────────────────────────

function SplashScreen({ onDone }) {
  const [animStep, setAnimStep] = useState(0);
  useEffect(() => {
    const t1 = setTimeout(() => setAnimStep(1), 300);
    const t2 = setTimeout(() => setAnimStep(2), 800);
    const t3 = setTimeout(() => onDone(), 2600);
    return () => { clearTimeout(t1); clearTimeout(t2); clearTimeout(t3); };
  }, []);

  return (
    <div style={{ width: "100%", height: "100%", background: `linear-gradient(145deg, ${COLORS.saffron} 0%, ${COLORS.indigo} 100%)`, display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center", position: "relative", overflow: "hidden" }}>
      <MandalaCircle size={400} color="#FFFFFF" style={{ top: -80, right: -80, opacity: animStep >= 1 ? 0.12 : 0, transition: "opacity 1s ease" }} />
      <MandalaCircle size={250} color="#FFFFFF" style={{ bottom: -60, left: -60, opacity: animStep >= 1 ? 0.08 : 0, transition: "opacity 1.2s ease 0.3s" }} />
      <PaisleyAccent color={COLORS.gold} size={60} style={{ top: 80, left: 30, opacity: animStep >= 1 ? 0.3 : 0, transition: "opacity 0.8s ease 0.5s" }} />
      <PaisleyAccent color={COLORS.gold} size={40} style={{ bottom: 120, right: 40, opacity: animStep >= 1 ? 0.3 : 0, transition: "opacity 0.8s ease 0.7s" }} />

      <div style={{ transform: animStep >= 1 ? "scale(1) translateY(0)" : "scale(0.6) translateY(30px)", opacity: animStep >= 1 ? 1 : 0, transition: "all 0.7s cubic-bezier(0.34, 1.56, 0.64, 1)", textAlign: "center" }}>
        {/* Logo Icon */}
        <div style={{ width: 90, height: 90, borderRadius: 24, background: "rgba(255,255,255,0.15)", backdropFilter: "blur(12px)", border: `2px solid ${COLORS.gold}`, display: "flex", alignItems: "center", justifyContent: "center", margin: "0 auto 16px", boxShadow: `0 8px 32px rgba(0,0,0,0.2), inset 0 1px 0 rgba(255,255,255,0.3)` }}>
          <svg width="52" height="52" viewBox="0 0 52 52" fill="none">
            <path d="M16 28 C16 28 20 24 26 26 C32 28 36 24 36 24" stroke="white" strokeWidth="2.5" strokeLinecap="round" />
            <circle cx="14" cy="30" r="4" fill="white" opacity="0.9" />
            <circle cx="38" cy="22" r="4" fill="white" opacity="0.9" />
            <path d="M14 26 L14 20 C14 18 15 17 17 17 L24 17" stroke="white" strokeWidth="2" strokeLinecap="round" />
            <path d="M38 26 L38 32 C38 34 37 35 35 35 L28 35" stroke="white" strokeWidth="2" strokeLinecap="round" />
            <circle cx="26" cy="26" r="22" stroke={COLORS.gold} strokeWidth="1.5" opacity="0.6" />
            <circle cx="26" cy="26" r="18" stroke={COLORS.gold} strokeWidth="0.8" opacity="0.3" />
          </svg>
        </div>

        <div style={{ fontFamily: "'Trebuchet MS', sans-serif", fontSize: 32, fontWeight: 800, color: COLORS.white, letterSpacing: -0.5 }}>
          KaamConnect
        </div>
        <div style={{ fontSize: 18, color: COLORS.goldLight, fontWeight: 600, marginTop: 2 }}>
          काम कनेक्ट
        </div>
      </div>

      <div style={{ position: "absolute", bottom: 80, textAlign: "center", opacity: animStep >= 2 ? 1 : 0, transform: animStep >= 2 ? "translateY(0)" : "translateY(16px)", transition: "all 0.6s ease 0.3s" }}>
        <div style={{ color: "rgba(255,255,255,0.9)", fontSize: 15, fontWeight: 500, letterSpacing: 0.3 }}>
          "Kaam Karo, Badhte Raho"
        </div>
        <div style={{ color: "rgba(255,255,255,0.6)", fontSize: 12, marginTop: 4 }}>
          काम करो, बढ़ते रहो
        </div>
      </div>
    </div>
  );
}

function OnboardingScreen({ onDone }) {
  const [step, setStep] = useState(0);
  const slides = [
    { emoji: "🏠", title: "Koi bhi Kaam, Ek App", titleHi: "कोई भी काम, एक ऐप", desc: "Home repair, tutoring, cleaning, photography — everything on KaamConnect.", color: COLORS.saffron },
    { emoji: "✅", title: "Local Professionals", titleHi: "स्थानीय पेशेवर", desc: "Verified workers near you with real reviews from real customers in your city.", color: COLORS.indigo },
    { emoji: "🔒", title: "Safe Payment, Guaranteed", titleHi: "सुरक्षित भुगतान", desc: "Pay only when you're satisfied. UPI, Cards, Wallets — all supported.", color: "#2A7A3B" },
  ];
  const s = slides[step];
  return (
    <div style={{ width: "100%", height: "100%", background: COLORS.ivory, display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "space-between", padding: "60px 28px 48px", position: "relative" }}>
      <button onClick={onDone} style={{ position: "absolute", top: 52, right: 24, color: COLORS.textLight, fontSize: 14, background: "none", border: "none", cursor: "pointer", fontWeight: 600 }}>Skip</button>

      <div style={{ flex: 1, display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center", gap: 24 }}>
        <div style={{ width: 140, height: 140, borderRadius: 40, background: `${s.color}18`, border: `2px solid ${s.color}30`, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 64 }}>
          {s.emoji}
        </div>
        <div style={{ textAlign: "center" }}>
          <div style={{ fontSize: 22, fontWeight: 800, color: COLORS.textDark, marginBottom: 6 }}>{s.title}</div>
          <div style={{ fontSize: 16, color: s.color, fontWeight: 600, marginBottom: 14 }}>{s.titleHi}</div>
          <div style={{ fontSize: 14, color: COLORS.textMid, lineHeight: 1.7, maxWidth: 280 }}>{s.desc}</div>
        </div>
      </div>

      <div style={{ width: "100%", display: "flex", flexDirection: "column", alignItems: "center", gap: 24 }}>
        <div style={{ display: "flex", gap: 8 }}>
          {slides.map((_, i) => <div key={i} style={{ width: i === step ? 24 : 8, height: 8, borderRadius: 4, background: i === step ? s.color : `${s.color}40`, transition: "all 0.3s ease" }} />)}
        </div>
        <button onClick={() => step < 2 ? setStep(step + 1) : onDone()} style={{ width: "100%", padding: "16px", borderRadius: 16, background: `linear-gradient(135deg, ${COLORS.saffron}, ${COLORS.saffronDark})`, color: COLORS.white, fontSize: 16, fontWeight: 700, border: "none", cursor: "pointer", boxShadow: `0 6px 20px ${COLORS.saffron}50` }}>
          {step < 2 ? "Aage Badho →" : "Shuru Karein 🚀"}
        </button>
      </div>
    </div>
  );
}

function LoginScreen({ onDone }) {
  const [phone, setPhone] = useState("");
  const [role, setRole] = useState("customer");
  const [lang, setLang] = useState("hi");
  return (
    <div style={{ width: "100%", height: "100%", background: COLORS.ivory, display: "flex", flexDirection: "column", position: "relative", overflow: "hidden" }}>
      <div style={{ background: `linear-gradient(160deg, ${COLORS.indigo} 0%, ${COLORS.indigoDark} 100%)`, padding: "60px 28px 48px", position: "relative", overflow: "hidden" }}>
        <MandalaCircle size={280} color="#FFFFFF" style={{ top: -80, right: -80, opacity: 0.08, position: "absolute" }} />
        <button onClick={() => setLang(lang === "hi" ? "en" : "hi")} style={{ position: "absolute", top: 52, right: 20, background: "rgba(255,255,255,0.15)", border: "1px solid rgba(255,255,255,0.3)", borderRadius: 20, padding: "5px 14px", color: COLORS.white, fontSize: 13, cursor: "pointer", fontWeight: 600 }}>
          {lang === "hi" ? "EN" : "हिं"}
        </button>
        <div style={{ fontSize: 28, fontWeight: 800, color: COLORS.white, marginBottom: 6 }}>KaamConnect</div>
        <div style={{ fontSize: 16, color: COLORS.goldLight, fontWeight: 500 }}>{lang === "hi" ? "स्वागत है! लॉगिन करें" : "Welcome! Sign In"}</div>
      </div>

      <div style={{ flex: 1, padding: "32px 28px", display: "flex", flexDirection: "column", gap: 24 }}>
        {/* Role Toggle */}
        <div style={{ background: COLORS.border, borderRadius: 14, padding: 4, display: "flex" }}>
          {["customer", "professional"].map(r => (
            <button key={r} onClick={() => setRole(r)} style={{ flex: 1, padding: "10px", borderRadius: 11, background: role === r ? COLORS.white : "transparent", border: "none", cursor: "pointer", fontSize: 13, fontWeight: role === r ? 700 : 500, color: role === r ? COLORS.saffron : COLORS.textMid, boxShadow: role === r ? "0 2px 8px rgba(0,0,0,0.1)" : "none", transition: "all 0.2s ease" }}>
              {r === "customer" ? (lang === "hi" ? "🏠 Customer" : "🏠 Customer") : (lang === "hi" ? "💼 Professional" : "💼 Professional")}
            </button>
          ))}
        </div>

        <div>
          <label style={{ fontSize: 13, fontWeight: 600, color: COLORS.textMid, display: "block", marginBottom: 8 }}>
            {lang === "hi" ? "मोबाइल नंबर" : "Mobile Number"}
          </label>
          <div style={{ display: "flex", background: COLORS.white, borderRadius: 14, border: `1.5px solid ${COLORS.border}`, overflow: "hidden", boxShadow: "0 2px 8px rgba(0,0,0,0.04)" }}>
            <div style={{ padding: "14px 14px", background: COLORS.ivoryDark, borderRight: `1px solid ${COLORS.border}`, fontSize: 14, color: COLORS.textMid, fontWeight: 600, whiteSpace: "nowrap" }}>🇮🇳 +91</div>
            <input value={phone} onChange={e => setPhone(e.target.value.replace(/\D/g, "").slice(0, 10))} placeholder={lang === "hi" ? "अपना नंबर दर्ज करें" : "Enter your number"} style={{ flex: 1, padding: "14px 16px", border: "none", outline: "none", fontSize: 15, color: COLORS.textDark, background: "transparent" }} />
          </div>
        </div>

        <button onClick={onDone} style={{ width: "100%", padding: "16px", borderRadius: 16, background: phone.length === 10 ? `linear-gradient(135deg, ${COLORS.saffron}, ${COLORS.saffronDark})` : COLORS.border, color: phone.length === 10 ? COLORS.white : COLORS.textLight, fontSize: 16, fontWeight: 700, border: "none", cursor: phone.length === 10 ? "pointer" : "default", boxShadow: phone.length === 10 ? `0 6px 20px ${COLORS.saffron}50` : "none", transition: "all 0.3s ease" }}>
          {lang === "hi" ? "OTP भेजें →" : "Send OTP →"}
        </button>

        <div style={{ textAlign: "center", color: COLORS.textLight, fontSize: 12, lineHeight: 1.6 }}>
          {lang === "hi" ? "जारी रखने पर आप हमारी शर्तें मानते हैं" : "By continuing you agree to our Terms & Privacy Policy"}
        </div>

        {/* Social divider */}
        <div style={{ display: "flex", alignItems: "center", gap: 12 }}>
          <div style={{ flex: 1, height: 1, background: COLORS.border }} />
          <span style={{ fontSize: 12, color: COLORS.textLight, whiteSpace: "nowrap" }}>ya phir / or</span>
          <div style={{ flex: 1, height: 1, background: COLORS.border }} />
        </div>
        <button style={{ width: "100%", padding: "14px", borderRadius: 14, background: COLORS.white, border: `1.5px solid ${COLORS.border}`, display: "flex", alignItems: "center", justifyContent: "center", gap: 10, fontSize: 14, fontWeight: 600, color: COLORS.textDark, cursor: "pointer" }}>
          <span style={{ fontSize: 18 }}>G</span> Google se jaari rakhe
        </button>
      </div>
    </div>
  );
}

function HomeCustomer({ setScreen }) {
  const [searchHi, setSearchHi] = useState(true);
  useEffect(() => {
    const t = setInterval(() => setSearchHi(h => !h), 2500);
    return () => clearInterval(t);
  }, []);

  return (
    <div style={{ width: "100%", height: "100%", background: COLORS.ivory, display: "flex", flexDirection: "column", overflow: "hidden" }}>
      {/* Top Bar */}
      <div style={{ background: `linear-gradient(135deg, ${COLORS.indigo} 0%, ${COLORS.indigoDark} 100%)`, padding: "48px 20px 20px", position: "relative", overflow: "hidden" }}>
        <MandalaCircle size={200} color="#FFFFFF" style={{ top: -60, right: -40, opacity: 0.07, position: "absolute" }} />
        <div style={{ display: "flex", alignItems: "center", justifyContent: "space-between", marginBottom: 16, position: "relative" }}>
          <div style={{ display: "flex", alignItems: "center", gap: 6 }}>
            <span style={{ fontSize: 16 }}>📍</span>
            <div>
              <div style={{ fontSize: 11, color: "rgba(255,255,255,0.6)" }}>Aapka shehar</div>
              <div style={{ fontSize: 14, fontWeight: 700, color: COLORS.white }}>Jaipur, Rajasthan ▾</div>
            </div>
          </div>
          <div style={{ fontSize: 20, fontWeight: 800, color: COLORS.white }}>KaamConnect</div>
          <div style={{ width: 36, height: 36, borderRadius: 12, background: "rgba(255,255,255,0.15)", display: "flex", alignItems: "center", justifyContent: "center", cursor: "pointer", fontSize: 16 }}>🔔</div>
        </div>
        {/* Search */}
        <div onClick={() => setScreen("search")} style={{ background: COLORS.white, borderRadius: 14, padding: "13px 16px", display: "flex", alignItems: "center", gap: 10, cursor: "pointer", boxShadow: "0 4px 16px rgba(0,0,0,0.15)" }}>
          <span style={{ fontSize: 18 }}>🔍</span>
          <span style={{ flex: 1, fontSize: 14, color: COLORS.textLight, transition: "all 0.3s ease" }}>
            {searchHi ? "Kya chahiye aapko?" : "What do you need?"}
          </span>
          <div style={{ width: 32, height: 32, borderRadius: 10, background: `${COLORS.saffron}20`, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 14 }}>🎤</div>
        </div>
      </div>

      <div style={{ flex: 1, overflowY: "auto", padding: "20px 16px", display: "flex", flexDirection: "column", gap: 24 }}>
        {/* Categories */}
        <div>
          <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: 14 }}>
            <div style={{ fontSize: 16, fontWeight: 800, color: COLORS.textDark }}>Kya dhundh rahe hain?</div>
            <span style={{ fontSize: 13, color: COLORS.saffron, fontWeight: 600, cursor: "pointer" }}>Sab dekho</span>
          </div>
          <div style={{ display: "grid", gridTemplateColumns: "repeat(4, 1fr)", gap: 10 }}>
            {categories.map((cat, i) => (
              <button key={i} onClick={() => setScreen("post-job")} style={{ background: COLORS.white, borderRadius: 14, padding: "12px 6px", display: "flex", flexDirection: "column", alignItems: "center", gap: 6, border: `1px solid ${COLORS.border}`, cursor: "pointer", boxShadow: "0 2px 8px rgba(0,0,0,0.04)" }}>
                <div style={{ width: 40, height: 40, borderRadius: 12, background: `${cat.color}18`, display: "flex", alignItems: "center", justifyContent: "center", fontSize: 20 }}>{cat.icon}</div>
                <span style={{ fontSize: 10, fontWeight: 700, color: COLORS.textDark, textAlign: "center", lineHeight: 1.3 }}>{cat.label}</span>
                <span style={{ fontSize: 9, color: COLORS.textLight, textAlign: "center" }}>{cat.labelHi}</span>
              </button>
            ))}
          </div>
        </div>

        {/* Near You */}
        <div>
          <div style={{ display: "flex", justifyContent: "space-betwee
