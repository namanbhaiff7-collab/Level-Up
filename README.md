import { useState, useEffect } from "react";

// Sung Jinwoo Style System Backend Logic const chaptersDB = { Physics: ["Kinematics", "Laws of Motion"], Chemistry: ["Mole Concept", "Atomic Structure"], Maths: ["Quadratic Equations", "Trigonometry"] };

function generateQuests() { return [ { id: 1, title: "Solve 50 Questions", reward: 100, done: false }, { id: 2, title: "Complete 1 Chapter", reward: 150, done: false }, { id: 3, title: "Do 25 PYQs", reward: 120, done: false } ]; }

export default function App() { const [xp, setXp] = useState(0); const [level, setLevel] = useState(1); const [rank, setRank] = useState("E Rank"); const [quests, setQuests] = useState(generateQuests()); const [penalty, setPenalty] = useState(false);

// Rank system like Solo Leveling useEffect(() => { if (xp > 2000) setRank("S Rank"); else if (xp > 1500) setRank("A Rank"); else if (xp > 1000) setRank("B Rank"); else if (xp > 500) setRank("C Rank"); else if (xp > 200) setRank("D Rank"); else setRank("E Rank"); }, [xp]);

// Daily penalty system useEffect(() => { const timer = setTimeout(() => { setPenalty(true); alert("You failed today's mission. Penalty activated: Double workload."); }, 1000 * 60 * 60 * 24);

return () => clearTimeout(timer);

}, []);

const completeQuest = (id) => { const updated = quests.map((q) => { if (q.id === id && !q.done) { setXp((prev) => prev + q.reward); return { ...q, done: true }; } return q; }); setQuests(updated); };

const gainXP = (amount) => { setXp((prev) => { const newXp = prev + amount; if (newXp >= level * 200) { setLevel((l) => l + 1); } return newXp; }); };

return ( <div className="p-6"> <h1 className="text-3xl font-bold">⚔️ JEE SOLO LEVELING SYSTEM</h1>

<div className="mt-4 border p-4 rounded">
    <p>Rank: {rank}</p>
    <p>Level: {level}</p>
    <p>XP: {xp}</p>
    {penalty && <p className="text-red-500">Penalty Active: Work Harder</p>}
  </div>

  <div className="mt-6">
    <h2 className="text-xl font-semibold">Daily Quests</h2>
    {quests.map((q) => (
      <div key={q.id} className="border p-3 mt-2 rounded">
        <p>{q.title}</p>
        <p>Reward: {q.reward} XP</p>
        <button
          disabled={q.done}
          onClick={() => completeQuest(q.id)}
          className="bg-green-500 text-white px-3 py-1 mt-2 rounded"
        >
          {q.done ? "Completed" : "Complete Quest"}
        </button>
      </div>
    ))}
  </div>

  <div className="mt-6">
    <h2 className="text-xl font-semibold">Grind Actions</h2>
    <button
      onClick={() => gainXP(10)}
      className="bg-blue-500 text-white px-3 py-1 mr-2 rounded"
    >Solve Question</button>

    <button
      onClick={() => gainXP(20)}
      className="bg-purple-500 text-white px-3 py-1 rounded"
    >Solve PYQ</button>
  </div>

  <div className="mt-6">
    <h2 className="text-xl font-semibold">Subjects</h2>
    {Object.keys(chaptersDB).map((sub) => (
      <div key={sub} className="mt-2">
        <p className="font-medium">{sub}</p>
        <ul className="ml-4 list-disc">
          {chaptersDB[sub].map((c) => (
            <li key={c}>{c}</li>
          ))}
        </ul>
      </div>
    ))}
  </div>
</div>

); }
