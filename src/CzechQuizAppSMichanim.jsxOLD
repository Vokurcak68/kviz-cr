import React, { useEffect, useMemo, useState } from "react";

// Kvíz o ČR pro IT tým – single-file React komponenta
// Vlastnosti:
// - Otázky se kladou po jedné
// - Za každou špatnou odpověď se aktivuje 10s penalizace (blokace odpovědí)
// - Volitelný počet otázek, náhodné pořadí, klávesové zkratky (1–4)
// - Čisté UI s Tailwind (stačí mít Tailwind v hostiteli)
// - Snadno rozšiřitelná sada otázek

// ======= Datový model otázek =======
const DEFAULT_QUESTION_BANK = [
  {
    q: "Jaké je hlavní město České republiky?",
    options: ["Brno", "Praha", "Ostrava", "Plzeň"],
    answer: 1,
    explanation: "Hlavním městem je Praha.",
  },
  {
    q: "Jaká měna se používá v České republice?",
    options: ["Euro", "Česká koruna", "Zlotý", "Forint"],
    answer: 1,
    explanation: "Oficiální měnou je česká koruna (CZK).",
  },
  {
    q: "Která řeka protéká Prahou?",
    options: ["Labe", "Morava", "Vltava", "Odra"],
    answer: 2,
    explanation: "Prahou protéká Vltava.",
  },
  {
    q: "Nejvyšší hora České republiky je…",
    options: ["Lysá hora", "Sněžka", "Radhošť", "Praděd"],
    answer: 1,
    explanation: "Sněžka (1603 m n. m.) v Krkonoších.",
  },
  {
    q: "Kolik krajů má Česká republika (včetně Prahy)?",
    options: ["10", "13", "14", "15"],
    answer: 2,
    explanation: "ČR má 14 krajů včetně hl. m. Prahy.",
  },
  {
    q: "Které město je známé pivovarem Pilsner Urquell?",
    options: ["České Budějovice", "Plzeň", "Žatec", "Jihlava"],
    answer: 1,
    explanation: "Pilsner Urquell pochází z Plzně.",
  },
  {
    q: "Jak se jmenuje nejdelší česká řeka (měřená v rámci ČR)?",
    options: ["Labe", "Morava", "Dyje", "Vltava"],
    answer: 3,
    explanation: "Nejdelší je Vltava.",
  },
  {
    q: "Který hrad je považován za největší hradní komplex na světě?",
    options: ["Karlštejn", "Pražský hrad", "Křivoklát", "Hluboká"],
    answer: 1,
    explanation: "Pražský hrad je jedním z největších hradních komplexů.",
  },
  {
    q: "Které město je známé tradičním lázeňstvím a filmovým festivalem?",
    options: ["Luhačovice", "Karlovy Vary", "Teplice", "Mariánské Lázně"],
    answer: 1,
    explanation: "Karlovy Vary hostí Mezinárodní filmový festival Karlovy Vary.",
  },
  {
    q: "Který český vědec je spojen se základy genetiky (dědičnost hrachu)?",
    options: ["Jan Evangelista Purkyně", "Jaroslav Heyrovský", "Gregor Johann Mendel", "Otto Wichterle"],
    answer: 2,
    explanation: "Gregor Johann Mendel zformuloval zákony dědičnosti.",
  },
  {
    q: "Které město je známé jako centrum jihomoravského vinařství?",
    options: ["Znojmo", "Třeboň", "Liberec", "Pardubice"],
    answer: 0,
    explanation: "Znojmo a okolí jsou proslulé vínem.",
  },
  {
    q: "Oficiální jazyk České republiky je…",
    options: ["Slovenština", "Polština", "Němčina", "Čeština"],
    answer: 3,
    explanation: "Oficiálním jazykem je čeština.",
  },
  {
    q: "Které české město je proslulé kostkami cukru a zámkem s barokní zahradou?",
    options: ["Kroměříž", "Kutná Hora", "Pardubice", "Letohrad"],
    answer: 2,
    explanation: "Pardubice jsou známé i perníkem; zámek a zahrada jsou v Kroměříži – ale otázka míří na cukrovarnictví v Pardubicích (spojené s rafinerií cukru).",
  },
  {
    q: "Který český vynálezce je spojen s kontaktními čočkami?",
    options: ["Otto Wichterle", "Prokop Diviš", "František Křižík", "Čeněk Karel"],
    answer: 0,
    explanation: "Otto Wichterle vyvinul měkké kontaktní čočky.",
  },
  {
    q: "Které město je na soutoku Labe a Orlice?",
    options: ["Kolín", "Hradec Králové", "Mělník", "Litoměřice"],
    answer: 1,
    explanation: "Hradec Králové.",
  },
  {
    q: "Jaké je mezinárodní vozidlové označení České republiky?",
    options: ["CZ", "CR", "CS", "CE"],
    answer: 0,
    explanation: "Značka je CZ.",
  },
  {
    q: "Které město je známé kostnicí v Sedlci a výrobou stříbra?",
    options: ["Kutná Hora", "Jihlava", "Příbram", "Kladno"],
    answer: 0,
    explanation: "Kutná Hora.",
  },
  {
    q: "Který národní park leží na jihu Čech u hranic s Rakouskem a Německem?",
    options: ["Krkonoše", "Podyjí", "Šumava", "České Švýcarsko"],
    answer: 2,
    explanation: "Národní park Šumava.",
  },
  {
    q: "Které město je domovem brněnského automotodromu (Masarykův okruh)?",
    options: ["Brno", "Most", "Třebíč", "Zlín"],
    answer: 0,
    explanation: "Masarykův okruh je u Brna.",
  },
  {
    q: "Které tradiční české jídlo obsahuje houskový knedlík, vepřové a zelí?",
    options: ["Svíčková", "Vepřo knedlo zelo", "Guláš", "Moravský vrabec"],
    answer: 1,
    explanation: "Vepřo knedlo zelo.",
  },
  {
    q: "Které město je sídlem Technické univerzity v Liberci?",
    options: ["Liberec", "Ústí nad Labem", "Ostrava", "Opava"],
    answer: 0,
    explanation: "Jak název napovídá – v Liberci.",
  },
  {
    q: "Kde se nachází kostel sv. Barbory – památka UNESCO?",
    options: ["Telč", "Kutná Hora", "Lednice", "Třebíč"],
    answer: 1,
    explanation: "V Kutné Hoře.",
  },
  {
    q: "Který průmyslový region je spojen s Třineckými železárnami?",
    options: ["Severní Čechy", "Moravskoslezský kraj", "Vysočina", "Jižní Čechy"],
    answer: 1,
    explanation: "Moravskoslezský kraj.",
  },
  {
    q: "Jaké je státní zřízení České republiky?",
    options: ["Federace", "Unitární republika", "Konstituční monarchie", "Autonomie"],
    answer: 1,
    explanation: "Unitární parlamentní republika.",
  },
  {
    q: "Které město je známé Baťovým kanálem a obuvnickou historií?",
    options: ["Zlín", "Uherské Hradiště", "Otrokovice", "Kroměříž"],
    answer: 0,
    explanation: "Zlín – Baťa.",
  },
  {
    q: "Které české město má přezdívku \"město perníku\"?",
    options: ["Pardubice", "Olomouc", "Hradec Králové", "Cheb"],
    answer: 0,
    explanation: "Pardubice.",
  },
  {
    q: "Který český chemik získal Nobelovu cenu za polarografii?",
    options: ["Jaroslav Heyrovský", "Emil Votoček", "Antonín Holý", "Rudolf Lukeš"],
    answer: 0,
    explanation: "Jaroslav Heyrovský.",
  },
  {
    q: "Které město je známé olomouckými tvarůžky?",
    options: ["Olomouc", "Loštice", "Prostějov", "Šternberk"],
    answer: 1,
    explanation: "Výroba je v Lošticích.",
  },
  {
    q: "Kde najdeme Pravčickou bránu?",
    options: ["Český ráj", "České Švýcarsko", "Moravský kras", "Pálava"],
    answer: 1,
    explanation: "Národní park České Švýcarsko.",
  },
  {
    q: "Které město hostí tradiční Velkou pardubickou steeplechase?",
    options: ["Pardubice", "Chuchle", "Karlovy Vary", "Most"],
    answer: 0,
    explanation: "Pardubice.",
  },
  {
    q: "Které město je známé výrobou skla a bižuterie v Jizerských horách?",
    options: ["Jablonec nad Nisou", "Semily", "Turnov", "Nový Bor"],
    answer: 0,
    explanation: "Jablonec nad Nisou.",
  },
  {
    q: "Které město je sídlem Univerzity Palackého?",
    options: ["Brno", "Olomouc", "Opava", "České Budějovice"],
    answer: 1,
    explanation: "Univerzita Palackého je v Olomouci.",
  },
];

// Utility – náhodné promíchání
function shuffleArray(arr) {
  const a = [...arr];
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}

// ======= Načítání externích otázek =======
function validateQuestions(payload) {
  const arr = Array.isArray(payload?.questions)
    ? payload.questions
    : (Array.isArray(payload) ? payload : []);
  const clean = [];
  for (const item of arr) {
    if (!item || typeof item.q !== 'string') continue;
    const options = Array.isArray(item.options) ? item.options.filter(o => typeof o === 'string') : [];
    const answer = Number.isInteger(item.answer) ? item.answer : -1;
    if (options.length < 2 || answer < 0 || answer >= options.length) continue;
    clean.push({ q: item.q, options, answer, explanation: item.explanation || "" });
  }
  return clean;
}

export default function CzechQuizApp() {
  const [stage, setStage] = useState("setup"); // setup | quiz | results
  const [questionCount, setQuestionCount] = useState(10);
  const [useShuffle, setUseShuffle] = useState(true);

  const [order, setOrder] = useState([]); // indexy otázek
  const [currentIdx, setCurrentIdx] = useState(0);
  const [score, setScore] = useState(0);
  const [lockedUntil, setLockedUntil] = useState(0); // timestamp ms
  const [lockRemaining, setLockRemaining] = useState(0); // s
  const [history, setHistory] = useState([]);

  // Externí otázky (questions.json v /public) + možnost nahrát vlastní JSON
  const [externalQuestions, setExternalQuestions] = useState(null);
  const [loadingQuestions, setLoadingQuestions] = useState(true);
  const [loadError, setLoadError] = useState(null);

  useEffect(() => {
    (async () => {
      try {
        const res = await fetch('/questions.json', { cache: 'no-store' });
        if (!res.ok) throw new Error('HTTP ' + res.status);
        const data = await res.json();
        const qs = validateQuestions(data);
        if (qs.length > 0) setExternalQuestions(qs);
      } catch (e) {
        setLoadError(String(e));
      } finally {
        setLoadingQuestions(false);
      }
    })();
  }, []);

  async function handleFileUpload(e) {
    const file = e.target.files?.[0];
    if (!file) return;
    try {
      const text = await file.text();
      const data = JSON.parse(text);
      const qs = validateQuestions(data);
      if (qs.length === 0) throw new Error('Soubor neobsahuje validní otázky.');
      setExternalQuestions(qs);
      setQuestionCount((n) => Math.min(n, qs.length));
      setLoadError(null);
    } catch (err) {
      setLoadError(String(err));
      alert('Chyba při čtení JSON: ' + err);
    }
  }

  const sourceBank = externalQuestions && externalQuestions.length ? externalQuestions : DEFAULT_QUESTION_BANK; // záznamy odpovědí

  const PENALTY_SECONDS = 10;

  // Přepočet zbývajícího času zámku
  useEffect(() => {
    if (lockedUntil <= Date.now()) {
      setLockRemaining(0);
      return;
    }
    const tick = () => {
      const remaining = Math.max(0, Math.ceil((lockedUntil - Date.now()) / 1000));
      setLockRemaining(remaining);
    };
    tick();
    const t = setInterval(tick, 200);
    return () => clearInterval(t);
  }, [lockedUntil]);

  const questions = useMemo(() => {
    const base = [...sourceBank];
    const arranged = useShuffle ? shuffleArray(base) : base;
    return arranged.slice(0, Math.min(questionCount, arranged.length));
  }, [questionCount, useShuffle, sourceBank]);

  const current = questions[currentIdx];
  const progress = ((currentIdx) / Math.max(1, questions.length)) * 100;

  function startQuiz() {
    setOrder(questions.map((_, i) => i));
    setCurrentIdx(0);
    setScore(0);
    setHistory([]);
    setLockedUntil(0);
    setStage("quiz");
  }

  function handleAnswer(optIndex) {
    if (!current) return;
    if (Date.now() < lockedUntil) return; // uzamčeno

    const isCorrect = optIndex === current.answer;
    const record = {
      q: current.q,
      chosen: current.options[optIndex],
      correct: current.options[current.answer],
      ok: isCorrect,
      time: new Date().toISOString(),
    };
    setHistory((h) => [...h, record]);

    if (isCorrect) {
      setScore((s) => s + 1);
      // další otázka po krátké prodlevě pro vizuální odezvu
      setTimeout(() => {
        if (currentIdx + 1 < questions.length) {
          setCurrentIdx((i) => i + 1);
        } else {
          setStage("results");
        }
      }, 300);
    } else {
      const until = Date.now() + PENALTY_SECONDS * 1000;
      setLockedUntil(until);
    }
  }

  function resetAll() {
    setStage("setup");
    setCurrentIdx(0);
    setScore(0);
    setLockedUntil(0);
    setHistory([]);
  }

  // Klávesové zkratky 1–4
  useEffect(() => {
    const onKey = (e) => {
      if (stage !== "quiz") return;
      if (["1", "2", "3", "4"].includes(e.key)) {
        const idx = parseInt(e.key, 10) - 1;
        handleAnswer(idx);
      }
    };
    window.addEventListener("keydown", onKey);
    return () => window.removeEventListener("keydown", onKey);
  }, [stage, currentIdx, lockedUntil]);

  return (
    <div className="min-h-screen w-full bg-slate-50 text-slate-900 flex items-center justify-center p-6">
      <div className="w-full max-w-3xl">
        <div className="mb-6">
          <h1 className="text-3xl font-bold">Kvíz o České republice</h1>
          <p className="text-slate-600">Pro IT tým – otázky po jedné, 10s penalizace za špatnou odpověď.</p>
        </div>

        {stage === "setup" && (
          <div className="bg-white rounded-2xl shadow p-6 space-y-6">
            <div className="grid gap-4 sm:grid-cols-2">
              <label className="flex flex-col gap-2">
                <span className="text-sm text-slate-600">Počet otázek</span>
                <input
                  type="number"
                  min={5}
                  max={sourceBank.length}
                  value={questionCount}
                  onChange={(e) => setQuestionCount(Number(e.target.value))}
                  className="border rounded-xl px-3 py-2 focus:outline-none focus:ring w-full"
                />
                <span className="text-xs text-slate-500">Max {sourceBank.length}</span>
              </label>

              <label className="flex items-center gap-3">
                <input
                  type="checkbox"
                  checked={useShuffle}
                  onChange={(e) => setUseShuffle(e.target.checked)}
                  className="h-5 w-5"
                />
                <span>Míchat pořadí otázek</span>
              </label>
            </div>

            {/* Info o zdroji otázek + nahrání JSON */}
            <div className="rounded-xl bg-slate-50 p-3 text-sm text-slate-700 border">
              {loadingQuestions ? (
                <span>Načítám externí otázky z <code>/questions.json</code>…</span>
              ) : externalQuestions ? (
                <span>Načteno {externalQuestions.length} externích otázek z <code>/questions.json</code>.</span>
              ) : (
                <span>Externí otázky nenalezeny – používám vestavěnou sadu.</span>
              )}
              {loadError && (
                <div className="text-red-600 mt-1">Chyba načítání: {String(loadError)}</div>
              )}
            </div>

            <div className="grid gap-3 sm:grid-cols-2 items-start">
              <label className="flex flex-col gap-2">
                <span className="text-sm text-slate-600">Nahrát vlastní JSON</span>
                <input type="file" accept="application/json" onChange={handleFileUpload} className="border rounded-xl px-3 py-2" />
                <span className="text-xs text-slate-500">Soubor musí odpovídat schématu níže.</span>
              </label>
              <details className="text-sm text-left">
                <summary className="cursor-pointer text-slate-600">Formát <code>questions.json</code></summary>
                <pre className="mt-2 p-3 bg-slate-100 rounded-xl overflow-auto text-xs">{`
{
  "questions": [
    {
      "q": "Text otázky",
      "options": ["A", "B", "C", "D"],
      "answer": 1,
      "explanation": "Volitelné vysvětlení"
    }
  ]
}
`}</pre>
              </details>
            </div>

            <button
              onClick={startQuiz}
              className="px-5 py-3 rounded-2xl bg-slate-900 text-white hover:opacity-90 transition shadow"
            >
              Začít kvíz
            </button>
          </div>
        )}

        {stage === "quiz" && current && (
          <div className="bg-white rounded-2xl shadow p-6 space-y-6">
            {/* Progress */}
            <div className="w-full h-3 bg-slate-100 rounded-full overflow-hidden">
              <div
                className="h-3 bg-slate-900 transition-all"
                style={{ width: `${progress}%` }}
              />
            </div>
            <div className="text-sm text-slate-600">Otázka {currentIdx + 1} / {questions.length}</div>

            <h2 className="text-xl font-semibold">{current.q}</h2>

            <div className="grid gap-3">
              {current.options.map((opt, i) => {
                const disabled = Date.now() < lockedUntil;
                return (
                  <button
                    key={i}
                    onClick={() => handleAnswer(i)}
                    disabled={disabled}
                    className={
                      "w-full text-left border rounded-xl px-4 py-3 hover:bg-slate-50 transition " +
                      (disabled ? "opacity-50 cursor-not-allowed" : "")
                    }
                    aria-label={`Odpověď ${i + 1}`}
                    title={`Klávesa ${i + 1}`}
                  >
                    <div className="flex items-center gap-3">
                      <span className="inline-flex items-center justify-center w-7 h-7 rounded-lg border text-sm">
                        {i + 1}
                      </span>
                      <span>{opt}</span>
                    </div>
                  </button>
                );
              })}
            </div>

            {lockRemaining > 0 ? (
              <div className="mt-2 text-red-600 font-medium">
                Špatně. Počkejte ještě {lockRemaining}s a zkuste to znovu.
              </div>
            ) : (
              <div className="text-slate-500 text-sm">Vyberte odpověď (nebo klávesu 1–4).</div>
            )}

            {/* Tip / nápověda: zobraz vysvětlení po správné odpovědi v historii */}
            <details className="mt-4 group">
              <summary className="cursor-pointer text-sm text-slate-600 group-open:mb-2">Historie pokusů</summary>
              <ul className="space-y-2 text-sm max-h-48 overflow-auto pr-2">
                {history.slice().reverse().map((h, idx) => (
                  <li key={idx} className="border rounded-xl p-2">
                    <div className="flex items-center justify-between">
                      <span className={h.ok ? "text-green-700" : "text-red-700"}>
                        {h.ok ? "✓" : "✗"} {h.q}
                      </span>
                      <span className="text-xs text-slate-500">{new Date(h.time).toLocaleTimeString()}</span>
                    </div>
                    <div className="text-slate-700">Vaše: {h.chosen}</div>
                    {!h.ok && <div className="text-slate-500">Správně: {h.correct}</div>}
                  </li>
                ))}
              </ul>
            </details>
          </div>
        )}

        {stage === "results" && (
          <div className="bg-white rounded-2xl shadow p-6 space-y-6 text-center">
            <h2 className="text-2xl font-semibold">Hotovo!</h2>
            <p className="text-slate-700">Skóre: <span className="font-bold">{score}</span> / {questions.length}</p>
            <div className="flex gap-3 justify-center">
              <button
                onClick={startQuiz}
                className="px-5 py-3 rounded-2xl bg-slate-900 text-white hover:opacity-90 transition shadow"
              >
                Hrát znovu se stejným nastavením
              </button>
              <button
                onClick={resetAll}
                className="px-5 py-3 rounded-2xl border hover:bg-slate-50 transition shadow"
              >
                Změnit nastavení
              </button>
            </div>

            <details className="mt-2">
              <summary className="cursor-pointer text-sm text-slate-600">Zobrazit detaily</summary>
              <ul className="mt-2 text-left text-sm space-y-2 max-h-64 overflow-auto pr-2">
                {history.map((h, i) => (
                  <li key={i} className="border rounded-xl p-2">
                    <div className="font-medium">{h.q}</div>
                    <div>Vaše: {h.chosen} {h.ok ? "(správně)" : "(špatně)"}</div>
                    {!h.ok && <div>Správně: {h.correct}</div>}
                  </li>
                ))}
              </ul>
            </details>
          </div>
        )}

        <footer className="mt-6 text-center text-xs text-slate-500">
          Tip: Chcete týmové kolo s více hráči a střídáním? Napište mi a doplním režim \"týmy\".
        </footer>
      </div>
    </div>
  );
}
