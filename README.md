# space-login
A single-file React component (Tailwind + shadcn/ui + framer-motion friendly) Purpose: themed login UI "Luar Angkasa" for a repo demo.
How to use:
- Place this file in a React app (Vite / CRA / Next.js) that already has Tailwind configured.
- Install optional dependencies if you want the animations and shadcn components:
  - npm i framer-motion
  - (optional) shadcn/ui components imports in the code assume a project with shadcn setup
- This component is self-contained visually using Tailwind classes. You can adapt it to integrate with your auth backend.

Demo credentials (mock):
- email: demo@space.fox
- password: rocket123

Notes:
- Accessibility: labels, aria attributes, keyboard-focus styles are included.
- Responsiveness: designed mobile-first and scales up for desktop.
- Theme: animated stars & planet using pure CSS + subtle parallax.

*/

import React, { useState } from "react";
import { motion } from "framer-motion";

export default function SpaceLogin() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");
  const [success, setSuccess] = useState("");

  function validate() {
    if (!email) return "Email tidak boleh kosong.";
    // simple email regex
    const re = /^(([^<>()[\\]\\.,;:\\s@\"]+(\\.[^<>()[\\]\\.,;:\\s@\"]+)*)|(\".+\"))@(([^<>()[\\]\\.,;:\\s@\"]+\\.)+[^<>()[\\]\\.,;:\\s@\"]{2,})$/i;
    if (!re.test(email)) return "Masukkan email yang valid.";
    if (!password || password.length < 6) return "Password minimal 6 karakter.";
    return "";
  }

  const handleSubmit = async (e) => {
    e.preventDefault();
    setError("");
    setSuccess("");
    const v = validate();
    if (v) {
      setError(v);
      return;
    }
    setLoading(true);
    // mock API
    setTimeout(() => {
      setLoading(false);
      if (email.toLowerCase() === "demo@space.fox" && password === "rocket123") {
        setSuccess("Login berhasil — selamat datang, Fox! 🚀");
      } else {
        setError("Email atau password salah. Coba lagi.");
      }
    }, 1100);
  };

  return (
    <div className="min-h-screen bg-gradient-to-b from-black via-slate-900 to-[#07021a] text-slate-100 flex items-center justify-center p-6">
      {/* animated background layer */}
      <div className="absolute inset-0 pointer-events-none -z-10 overflow-hidden">
        <div className="stars w-full h-full">
          {/* CSS will create small stars */}
        </div>
        <div className="planet -right-20 -bottom-20 opacity-20" aria-hidden="true" />
      </div>

      <motion.div
        initial={{ y: 30, opacity: 0 }}
        animate={{ y: 0, opacity: 1 }}
        transition={{ duration: 0.6 }}
        className="relative z-10 w-full max-w-2xl bg-gradient-to-br from-slate-800/60 to-slate-900/60 backdrop-blur-md border border-slate-700/40 rounded-2xl p-6 shadow-2xl"
      >
        <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
          {/* Left: Visual */}
          <div className="flex flex-col gap-4 justify-center items-start p-4 md:p-6">
            <div className="flex items-center gap-3">
              <div className="w-12 h-12 rounded-full bg-gradient-to-tr from-indigo-400 to-rose-400 flex items-center justify-center text-black font-bold text-lg">
                🚀
              </div>
              <div>
                <h1 className="text-2xl font-semibold leading-tight">Login — Mission Control</h1>
                <p className="text-sm text-slate-300/90">Masuk untuk melanjutkan perjalanan luar angkasa kamu.</p>
              </div>
            </div>

            <div className="mt-4 text-slate-300 text-sm">
              <p className="mb-2">Tips cepat:</p>
              <ul className="list-disc list-inside space-y-1">
                <li>Pakai email demo: <span className="font-medium text-slate-100">demo@space.fox</span></li>
                <li>Password contoh: <span className="font-medium text-slate-100">rocket123</span></li>
                <li>Atau hubungkan backend autentikasi untuk login nyata.</li>
              </ul>
            </div>

            <div className="mt-6 w-full">
              <div className="rounded-lg overflow-hidden">
                <div className="w-full h-36 bg-[url('https://images.unsplash.com/photo-1446776811953-b23d57bd21aa?q=80&w=1600&auto=format&fit=crop&ixlib=rb-4.0.3&s=...')] bg-cover bg-center opacity-90">
                  {/* Decorative starfield image fallback */}
                </div>
              </div>
            </div>
          </div>

          {/* Right: Form */}
          <div className="p-4 md:p-6 flex flex-col justify-center">
            <form onSubmit={handleSubmit} className="space-y-4">
              <label className="block">
                <span className="text-sm text-slate-300">Email</span>
                <input
                  type="email"
                  value={email}
                  onChange={(e) => setEmail(e.target.value)}
                  className="mt-1 block w-full rounded-md bg-slate-800 border border-slate-700 px-3 py-2 placeholder:text-slate-400 outline-none focus:ring-2 focus:ring-indigo-400"
                  placeholder="email@contoh.com"
                  aria-label="Email"
                  required
                />
              </label>

              <label className="block">
                <div className="flex justify-between text-sm text-slate-300">
                  <span>Password</span>
                  <a href="#" className="text-xs text-indigo-300 hover:underline">Lupa?</a>
                </div>
                <input
                  type="password"
                  value={password}
                  onChange={(e) => setPassword(e.target.value)}
                  className="mt-1 block w-full rounded-md bg-slate-800 border border-slate-700 px-3 py-2 placeholder:text-slate-400 outline-none focus:ring-2 focus:ring-indigo-400"
                  placeholder="password"
                  aria-label="Password"
                  required
                />
              </label>

              {error && <div className="text-sm text-rose-400">{error}</div>}
              {success && <div className="text-sm text-emerald-400">{success}</div>}

              <div className="flex items-center gap-3">
                <button
                  type="submit"
                  disabled={loading}
                  className="inline-flex items-center justify-center gap-2 rounded-lg bg-indigo-500 hover:bg-indigo-600 px-4 py-2 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-indigo-400 disabled:opacity-60"
                >
                  {loading ? (
                    <svg className="w-4 h-4 animate-spin text-white" viewBox="0 0 24 24">
                      <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4"></circle>
                      <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8v8z"></path>
                    </svg>
                  ) : (
                    "Masuk"
                  )}
                </button>

                <button
                  type="button"
                  onClick={() => {
                    setEmail("demo@space.fox");
                    setPassword("rocket123");
                    setError("");
                    setSuccess("");
                  }}
                  className="text-sm px-3 py-2 rounded-md border border-slate-700 hover:bg-slate-800/40"
                >
                  Coba Demo
                </button>
              </div>

              <p className="text-xs text-slate-400">Belum punya akun? <a href="#" className="text-indigo-300 hover:underline">Daftar</a></p>
            </form>

            <div className="mt-6 text-xs text-slate-500">Tip: lepaskan "pemberat" (beban)mu — bergabung dengan misi terasa lebih mudah tanpa beban berlebih.</div>
          </div>
        </div>

        {/* Footer small */}
        <div className="mt-6 text-center text-xs text-slate-500">© {new Date().getFullYear()} Fox Mission • UI demo</div>
      </motion.div>

      {/* Inline styles for starfield & planet (Tailwind can't express some custom animations easily) */}
      <style>{`
        .stars::before, .stars::after {
          content: '';
          position: absolute;
          inset: 0;
          background-image: radial-gradient(white 1px, transparent 1px);
          background-size: 3px 3px;
          opacity: 0.06;
          animation: twinkle 12s linear infinite;
        }
        .stars::after {
          background-size: 6px 6px;
          opacity: 0.03;
          transform: translateY(1000px);
          animation-duration: 24s;
        }
        @keyframes twinkle {
          0% { transform: translateY(0) rotate(0deg); }
          100% { transform: translateY(-1000px) rotate(360deg); }
        }
        .planet {
          position: absolute;
          width: 420px;
          height: 420px;
          border-radius: 50%;
          background: radial-gradient(circle at 30% 30%, rgba(255,180,120,0.12), rgba(0,0,0,0.06) 40%), conic-gradient(from 45deg, rgba(120,90,200,0.12), rgba(90,120,200,0.08));
          filter: blur(40px);
          transform: scale(1.05) translateZ(0);
          z-index: -1;
        }
        @media (max-width: 640px) {
          .planet { width: 260px; height: 260px; right: -40px; bottom: -40px; }
        }
      `}</style>
    </div>
  );
}
