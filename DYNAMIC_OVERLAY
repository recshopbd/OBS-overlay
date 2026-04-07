"use client";
import { useEffect, useState } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { db } from '@/lib/firebase'; // Your firebase config
import { doc, onSnapshot } from 'firebase/firestore';

export default function MatchOverlay({ params }) {
  const [matchData, setMatchData] = useState(null);

  useEffect(() => {
    // Real-time listener for the match
    const unsub = onSnapshot(doc(db, "matches", params.matchId), (doc) => {
      setMatchData(doc.data());
    });
    return () => unsub();
  }, [params.matchId]);

  if (!matchData) return null;

  return (
    <div className="w-[1920px] h-[1080px] bg-transparent text-white font-sans overflow-hidden relative">
      
      {/* 1. TOP BAR: RecArena Branding & Timer */}
      <div className="absolute top-0 left-0 w-full h-20 flex justify-center items-center">
        <div className="bg-black/60 backdrop-blur-md border-b-2 border-neon-green px-12 py-2 rounded-b-3xl shadow-[0_0_20px_rgba(57,255,20,0.3)]">
          <h1 className="text-3xl font-black tracking-tighter italic">
            REC<span className="text-neon-green">ARENA</span> LIVE
          </h1>
        </div>
      </div>

      {/* 2. PLAYER FRAMES (Layout changes based on 1v1 or 2v2) */}
      <div className="absolute inset-0 flex justify-between items-center px-10">
        {/* Player 1 Frame Area */}
        <div className="w-[850px] h-[480px] border-2 border-white/10 rounded-lg relative overflow-hidden">
             {/* This area is transparent in the browser so the OBS layer behind it shows through */}
        </div>

        {/* VS Badge */}
        <div className="flex flex-col items-center">
             <div className="text-6xl font-black text-neon-blue italic drop-shadow-lg">VS</div>
        </div>

        {/* Player 2 Frame Area */}
        <div className="w-[850px] h-[480px] border-2 border-white/10 rounded-lg relative">
        </div>
      </div>

      {/* 3. DYNAMIC NAMEPLATES (Glassmorphism) */}
      <div className="absolute bottom-20 w-full flex justify-around px-20">
        {/* P1 Nameplate */}
        <motion.div 
          initial={{ x: -100, opacity: 0 }}
          animate={{ x: 0, opacity: 1 }}
          className="w-80 h-16 bg-black/80 backdrop-blur-xl border-l-4 border-neon-green flex items-center px-6 rounded-r-xl shadow-2xl"
        >
          <img src={matchData.player1_photo} className="w-10 h-10 rounded-full border border-neon-green mr-4" />
          <span className="text-xl font-bold uppercase tracking-wider">{matchData.player1_name}</span>
        </motion.div>

        {/* P2 Nameplate */}
        <motion.div 
          initial={{ x: 100, opacity: 0 }}
          animate={{ x: 0, opacity: 1 }}
          className="w-80 h-16 bg-black/80 backdrop-blur-xl border-r-4 border-neon-blue flex items-center justify-end px-6 rounded-l-xl shadow-2xl"
        >
          <span className="text-xl font-bold uppercase tracking-wider mr-4">{matchData.player2_name}</span>
          <img src={matchData.player2_photo} className="w-10 h-10 rounded-full border border-neon-blue" />
        </motion.div>
      </div>

      {/* 4. TICKER / SPONSOR BAR */}
      <div className="absolute bottom-0 w-full h-10 bg-gradient-to-r from-transparent via-black/80 to-transparent flex items-center justify-center">
        <p className="text-xs uppercase tracking-[0.3em] text-white/50 animate-pulse">
          Next Match: {matchData.next_match_time} | Powered by <span className="text-neon-green">RecShop BD</span>
        </p>
      </div>
    </div>
  );
}
