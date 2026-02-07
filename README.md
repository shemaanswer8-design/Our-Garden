import { useState, useEffect } from "react";
import { Link } from "react-router-dom";
import { createPageUrl } from "./utils";
import { Menu, X, Leaf, ChevronRight } from "lucide-react";
import { motion, AnimatePresence } from "framer-motion";

export default function Layout({ children, currentPageName }) {
  const [scrolled, setScrolled] = useState(false);
  const [mobileOpen, setMobileOpen] = useState(false);

  useEffect(() => {
    const onScroll = () => setScrolled(window.scrollY > 20);
    window.addEventListener("scroll", onScroll);
    return () => window.removeEventListener("scroll", onScroll);
  }, []);

  useEffect(() => {
    setMobileOpen(false);
  }, [currentPageName]);

  const navLinks = [
    { label: "Home", page: "Home" },
    { label: "Our Boxes", page: "Boxes" },
    { label: "Custom Boxes", page: "CustomBoxes" },
    { label: "Blog", page: "Blog" },
  ];

  return (
    <div className="min-h-screen bg-[#FDF8F0] text-[#1E3A2B]">
      <style>{`
        :root {
          --garden-green: #3B5F4A;
          --garden-dark: #1E3A2B;
          --garden-cream: #FDF8F0;
          --garden-terracotta: #C4836A;
          --garden-sage: #8BA888;
          --garden-stone: #A09B93;
        }
        * { font-family: 'Inter', system-ui, -apple-system, sans-serif; }
        html { scroll-behavior: smooth; }
      `}</style>

      {/* Navigation */}
      <header
        className={`fixed top-0 left-0 right-0 z-50 transition-all duration-500 ${
          scrolled
            ? "bg-white/90 backdrop-blur-xl shadow-[0_1px_0_rgba(30,58,43,0.08)]"
            : "bg-transparent"
        }`}
      >
        <div className="max-w-7xl mx-auto px-6 lg:px-8">
          <div className="flex items-center justify-between h-20">
            {/* Logo */}
            <Link to={createPageUrl("Home")} className="flex items-center gap-2.5 group">
              <div className="w-9 h-9 rounded-full bg-[#3B5F4A] flex items-center justify-center group-hover:scale-105 transition-transform">
                <Leaf className="w-4.5 h-4.5 text-white" strokeWidth={2} />
              </div>
              <span className="text-xl font-semibold tracking-tight text-[#1E3A2B]">
                Our Garden
              </span>
            </Link>

            {/* Desktop Nav */}
            <nav className="hidden md:flex items-center gap-1">
              {navLinks.map((link) => (
                <Link
                  key={link.page}
                  to={createPageUrl(link.page)}
                  className={`px-4 py-2 rounded-full text-sm font-medium transition-all duration-300 ${
                    currentPageName === link.page
                      ? "bg-[#3B5F4A] text-white"
                      : "text-[#1E3A2B]/70 hover:text-[#1E3A2B] hover:bg-[#3B5F4A]/5"
                  }`}
                >
                  {link.label}
                </Link>
              ))}
            </nav>

            {/* Mobile Toggle */}
            <button
              onClick={() => setMobileOpen(!mobileOpen)}
