// === NIRMAN - Full Multi-Page Website with Modern UI and Chatbot ===

// === pages/index.js ===
import React from 'react';
import Link from 'next/link';
import dynamic from 'next/dynamic';
import { motion } from 'framer-motion';

const ChatbotComponent = dynamic(() => import('../components/Chatbot').then(mod => mod.default), { ssr: false });

const Home = () => {
  return (
    <div className="min-h-screen bg-gradient-to-br from-sky-50 to-indigo-100 text-gray-900">
      {/* Hero Section */}
      <section className="text-center py-20 px-4">
        <motion.h1 initial={{ opacity: 0, y: -40 }} animate={{ opacity: 1, y: 0 }} transition={{ duration: 1 }} className="text-5xl font-extrabold text-indigo-900">
          Smarter Home Construction, Powered by AI
        </motion.h1>
        <p className="text-lg mt-4 text-gray-600">
          Real-time tracking, AI design, Budget estimator, Eco-concrete
        </p>
        <div className="mt-6 space-x-4">
          <Link href="/waitlist"><button className="bg-indigo-600 text-white px-6 py-2 rounded-lg hover:bg-indigo-700">Join Waitlist</button></Link>
          <Link href="/features"><button className="bg-white text-indigo-600 px-6 py-2 border border-indigo-600 rounded-lg hover:bg-indigo-50">Explore Features</button></Link>
        </div>
      </section>

      {/* Image Grid with Scroll Popup */}
      <section className="p-10 grid grid-cols-1 md:grid-cols-2 gap-6">
        {["house1.jpg", "house2.jpg", "construction1.jpg", "construction2.jpg"].map((img, i) => (
          <motion.div key={i} whileInView={{ opacity: 1, y: 0 }} initial={{ opacity: 0, y: 50 }} transition={{ duration: 0.6 }}>
            <img src={`/images/${img}`} alt={img} className="rounded-xl shadow-xl w-full h-72 object-cover" />
          </motion.div>
        ))}
      </section>

      {/* About Us & Contact Section */}
      <footer className="bg-indigo-50 py-10 text-center text-gray-700">
        <h3 className="text-xl font-semibold mb-2">About NIRMAN</h3>
        <p className="max-w-2xl mx-auto mb-4">
          Founded by Chinmay Rout, NIRMAN is on a mission to simplify and modernize home construction in India using AI, sustainability, and user-first design.
        </p>
        <h4 className="text-lg font-medium">Contact & Support</h4>
        <p>Email: support@nirman.in | Instagram: @nirman | WhatsApp: +91-XXXX-XXX</p>
      </footer>

      {/* Chatbot (Nirmaan Assistant) */}
      <ChatbotComponent />
    </div>
  );
};

export default Home;

// === pages/waitlist.js ===
import React, { useState } from 'react';

const Waitlist = () => {
  const [formData, setFormData] = useState({ name: '', email: '', role: '' });
  const [success, setSuccess] = useState(false);

  const handleChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    const res = await fetch('/api/waitlist', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(formData),
    });
    if (res.ok) {
      setSuccess(true);
      setFormData({ name: '', email: '', role: '' });
    }
  };

  return (
    <div className="min-h-screen bg-white flex items-center justify-center p-6">
      <div className="max-w-lg w-full bg-indigo-50 p-8 rounded-lg shadow-md">
        <h2 className="text-2xl font-bold text-indigo-800 mb-6">Join the NIRMAN Waitlist</h2>
        {success ? (
          <p className="text-green-600 font-semibold">You're on the list! We'll be in touch soon 🚀</p>
        ) : (
          <form onSubmit={handleSubmit} className="space-y-4">
            <input type="text" name="name" placeholder="Your Name" value={formData.name} onChange={handleChange} required className="w-full p-3 rounded border border-indigo-300" />
            <input type="email" name="email" placeholder="Your Email" value={formData.email} onChange={handleChange} required className="w-full p-3 rounded border border-indigo-300" />
            <input type="text" name="role" placeholder="Your Role (e.g., Architect, Homeowner)" value={formData.role} onChange={handleChange} className="w-full p-3 rounded border border-indigo-300" />
            <button type="submit" className="w-full bg-indigo-600 text-white py-3 rounded hover:bg-indigo-700">Submit</button>
          </form>
        )}
      </div>
    </div>
  );
};

export default Waitlist;

// === Backend API: pages/api/waitlist.js ===

export const config = {
  api: {
    bodyParser: true,
  },
};

const waitlistHandler = async (req, res) => {
  if (req.method === "POST") {
    const { name, email, role } = req.body;
    console.log("Waitlist submission:", { name, email, role });
    return res.status(200).json({ success: true });
  }
  return res.status(405).json({ error: "Method not allowed" });
};

export default waitlistHandler;

// === components/Chatbot.js ===
import React from 'react';

const Chatbot = () => {
  return (
    <div className="fixed bottom-6 right-6 z-50">
      <iframe
        src="https://your-chatbot-widget-url.com"
        className="w-72 h-96 border-none rounded-xl shadow-lg"
        title="Nirmaan Assistant"
      ></iframe>
    </div>
  );
};

export default Chatbot;

// === Folder Structure Suggestion ===
// pages/
// ├── index.js (home page)
// ├── features.js
// ├── demo.js
// ├── waitlist.js
// └── api/
//     └── waitlist.js
// components/
// └── Chatbot.js
// public/images/ (include house1.jpg, house2.jpg, construction1.jpg, construction2.jpg)

// Note: Add Tailwind, Framer Motion, and deploy with chatbot integration.
