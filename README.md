import { useState, useEffect } from "react";

export default function FlagGame() {
  const [score, setScore] = useState(0);
  const [bestScore, setBestScore] = useState(
    parseInt(localStorage.getItem("bestScore")) || 0
  );
  const [theme, setTheme] = useState("dark");
  const [currentIndex, setCurrentIndex] = useState(0);
  const [answer, setAnswer] = useState("");
  const [selectedContinent, setSelectedContinent] = useState(null);

  const flags = [
    // Europe
    { name: "France", continent: "Europe", url: "https://flagcdn.com/w320/fr.png" },
    { name: "Allemagne", continent: "Europe", url: "https://flagcdn.com/w320/de.png" },
    { name: "Italie", continent: "Europe", url: "https://flagcdn.com/w320/it.png" },
    { name: "Espagne", continent: "Europe", url: "https://flagcdn.com/w320/es.png" },
    { name: "Royaume-Uni", continent: "Europe", url: "https://flagcdn.com/w320/gb.png" },
    { name: "Suède", continent: "Europe", url: "https://flagcdn.com/w320/se.png" },
    // Ajouter plus de pays ici selon ton besoin
    // Asie
    { name: "Chine", continent: "Asie", url: "https://flagcdn.com/w320/cn.png" },
    { name: "Japon", continent: "Asie", url: "https://flagcdn.com/w320/jp.png" },
    { name: "Inde", continent: "Asie", url: "https://flagcdn.com/w320/in.png" },
    // Ajouter plus de pays ici selon ton besoin
    // Afrique
    { name: "Afrique du Sud", continent: "Afrique", url: "https://flagcdn.com/w320/za.png" },
    { name: "Nigeria", continent: "Afrique", url: "https://flagcdn.com/w320/ng.png" },
    // Ajouter plus de pays ici selon ton besoin
    // Amérique
    { name: "États-Unis", continent: "Amérique", url: "https://flagcdn.com/w320/us.png" },
    { name: "Canada", continent: "Amérique", url: "https://flagcdn.com/w320/ca.png" },
    // Ajouter plus de pays ici selon ton besoin
    // Océanie
    { name: "Australie", continent: "Océanie", url: "https://flagcdn.com/w320/au.png" },
    { name: "Nouvelle-Zélande", continent: "Océanie", url: "https://flagcdn.com/w320/nz.png" },
    // Ajouter plus de pays ici selon ton besoin
  ];

  const continents = [
    "Europe", "Asie", "Afrique", "Amérique", "Océanie", "Antarctique"
  ];

  useEffect(() => {
    document.body.className = theme === "dark" ? "bg-black text-white" : "bg-white text-black";
  }, [theme]);

  const toggleTheme = () => {
    setTheme(theme === "dark" ? "light" : "dark");
  };

  const validateAnswer = () => {
    if (answer.trim().toLowerCase() === flags[currentIndex].name.toLowerCase()) {
      const newScore = score + 1;
      setScore(newScore);
      if (newScore > bestScore) {
        setBestScore(newScore);
        localStorage.setItem("bestScore", newScore);
      }
      nextFlag();
    } else {
      alert("Mauvaise réponse !");
    }
    setAnswer("");
  };

  const nextFlag = () => {
    setCurrentIndex((currentIndex + 1) % filteredFlags.length);
  };

  const filteredFlags = selectedContinent
    ? flags.filter((flag) => flag.continent === selectedContinent)
    : flags;

  return (
    <div className="flex flex-col items-center p-4">
      {/* Top Bar */}
      <div className="flex justify-between w-full max-w-lg items-center text-lg font-bold mb-4">
        <div>CDT.Pays</div>
        <div>Jeu du Drapeau</div>
        <button className="p-2 bg-white text-black rounded" onClick={toggleTheme}>
          {theme === "dark" ? "Mode clair" : "Mode sombre"}
        </button>
      </div>

      {/* Scoreboard */}
      <div className="text-lg font-bold">Score : {score}</div>
      <div className="text-lg font-bold mb-4">Meilleur Score : {bestScore}</div>

      {/* Flag */}
      <img
        src={filteredFlags[currentIndex].url}
        alt="Drapeau"
        className="w-96 h-60 border border-white my-4"
      />

      {/* Input Search */}
      <input
        type="text"
        className="w-80 p-2 text-center border rounded text-black"
        placeholder="Entrez le nom du pays..."
        value={answer}
        onChange={(e) => setAnswer(e.target.value)}
      />

      {/* Buttons */}
      <div className="flex gap-4 mt-4">
        <button className="p-2 bg-blue-500 text-white rounded" onClick={validateAnswer}>
          Valider
        </button>
        <button className="p-2 bg-gray-500 text-white rounded" onClick={nextFlag}>
          Passer
        </button>
      </div>

      {/* Continents */}
      <div className="flex gap-2 mt-4 flex-wrap justify-center">
        {continents.map((continent) => (
          <button
            key={continent}
            className="p-2 bg-green-500 text-white rounded"
            onClick={() => setSelectedContinent(continent)}
          >
            {continent}
          </button>
        ))}
      </div>
    </div>
  );
}
