<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Figma Assignment - Srikanth Portfolio</title>

  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    :root{
      --bg-dark: #0f1720;
      --card-dark: #1f2329;
      --muted: #9aa0a6;
    }
    body { 
      background: var(--bg-dark); 
      color: #e6eef8; 
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue";
      min-height: 100vh;
      padding: 20px;
    }
    .neumo { box-shadow: 8px 8px 20px rgba(2,6,23,0.8), -8px -8px 20px rgba(255,255,255,0.02); }
    .inner-shadow { box-shadow: inset 6px 6px 12px rgba(0,0,0,0.45), inset -4px -4px 8px rgba(255,255,255,0.02); }
    .scrollbar-hide::-webkit-scrollbar { display:none; }
  </style>
</head>
<body>

  <div id="root"></div>

  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

  <script type="text/babel">
  const { useState, useEffect, useRef } = React;

  function App(){
    return (
      <main className="flex justify-center items-start w-full min-h-screen">
        {/* Left empty space on large screens */}
        <div className="hidden lg:block w-1/2"></div>

        {/* Right side widgets */}
        <div className="w-full lg:w-1/2 flex flex-col space-y-10">
          <WidgetTabs />
          <GalleryWidget />
        </div>
      </main>
    );
  }

  function WidgetTabs(){
    const tabs = ['About Me','Experiences','Recommended'];
    const [active, setActive] = useState(0);

    return (
      <div className="rounded-3xl bg-[var(--card-dark)] neumo p-8 w-full">
        <div className="flex gap-6 items-center mb-6 justify-center">
          {tabs.map((t,i)=>(
            <button key={t}
              onClick={()=>setActive(i)}
              className={
                "px-6 py-3 rounded-full text-base font-semibold transition-all duration-150 "+
                (active===i ? 'bg-black text-white shadow-lg' : 'bg-transparent text-[var(--muted)]')
              }
              aria-pressed={active===i}
            >
              {t}
            </button>
          ))}
        </div>

        <div className="rounded-xl bg-[#22262b] p-8 max-h-72 overflow-auto text-base leading-7 text-gray-300">
          {active===0 && (
            <div>
              <h3 className="text-xl font-semibold mb-3">About Me</h3>
              <p>
                I’m <b>Srikanth Krishnan</b>, a motivated and curious B.Tech Information Technology student from SMV Engineering College, Puducherry (GPA: 8.56/10).  
                I seek opportunities to apply my technical knowledge while expanding my professional capabilities through challenging roles.  
              </p>
              <p className="mt-3 text-[var(--muted)]">
                My technical skills include <b>Java, Python, PHP, JavaScript, SQL,</b> and frameworks like <b>React.js, Node.js, TensorFlow,</b> and <b>Spring Boot</b>.  
                I’m passionate about full-stack web development, debugging, and AI-driven applications.
              </p>
              <p className="mt-3 text-[var(--muted)]">
                I’m driven to grow continuously, contribute meaningfully to organizational goals, and stay updated with modern technologies.
              </p>
            </div>
          )}
          {active===1 && (
            <div>
              <h4 className="font-semibold mb-2">Experience & Projects</h4>
              <ul className="list-disc list-inside text-[var(--muted)]">
                <li><b>Software Developer Intern</b> – Atharvo India Pvt. Ltd. (Aug–Sep 2024)</li>
                <li>Developed predictive ML applications and optimized pipelines using Python.</li>
                <li>Built the <b>PSCNN-BiLSTM-ARIMA</b> hybrid model for stock market forecasting with improved accuracy.</li>
                <li>Worked on full-stack mini projects using Node.js, MySQL, and React.js.</li>
              </ul>
            </div>
          )}
          {active===2 && (
            <div>
              <h4 className="font-semibold mb-2">Recommended</h4>
              <p className="text-[var(--muted)]">Books & Resources:</p>
              <ul className="list-disc list-inside text-[var(--muted)]">
                <li>Deep Learning with Python – François Chollet</li>
                <li>Clean Code – Robert C. Martin</li>
                <li>AI & ML Roadmap (TensorFlow, Scikit-learn, Pandas)</li>
                <li>Full-Stack Web Dev with React & Node.js</li>
              </ul>
            </div>
          )}
        </div>
      </div>
    );
  }

  function GalleryWidget(){
    const [images, setImages] = useState([
      "https://images.unsplash.com/photo-1504384308090-c894fdcc538d?w=800",
      "https://images.unsplash.com/photo-1508921912186-1d1a45ebb3c1?w=800",
      "https://images.unsplash.com/photo-1522202176988-66273c2fd55f?w=800",
      "https://images.unsplash.com/photo-1521790361206-5d47a2e7d9bf?w=800",
      "https://images.unsplash.com/photo-1503023345310-bd7c1de61c7d?w=800",
      "https://images.unsplash.com/photo-1532074205216-d0e1f4b87368?w=800"
    ]);
    const scroller = useRef(null);
    const fileRef = useRef(null);

    useEffect(()=>{
      const saved = localStorage.getItem('gallery_imgs_v3');
      if(saved) {
        try{ setImages(JSON.parse(saved)); } catch(e){}
      }
    },[]);
    useEffect(()=> localStorage.setItem('gallery_imgs_v3', JSON.stringify(images)), [images]);

    function addImage(e){
      const f = e.target.files && e.target.files[0];
      if(!f) return;
      const reader = new FileReader();
      reader.onload = ()=> setImages(prev => [reader.result, ...prev]);
      reader.readAsDataURL(f);
      e.target.value = null;
    }

    function scrollBy(dir = 1){
      if(scroller.current){
        scroller.current.scrollBy({ left: dir * 320, behavior: 'smooth' });
      }
    }

    return (
      <div className="rounded-3xl bg-[var(--card-dark)] neumo p-8 w-full">
        <div className="flex items-center justify-between mb-6">
          <div className="rounded-full bg-black px-6 py-3 text-white font-semibold text-lg">Gallery</div>

          <div className="flex items-center gap-4">
            <button onClick={()=>fileRef.current.click()} className="px-5 py-3 bg-[#2b3035] rounded-full inner-shadow text-sm font-medium">+ ADD IMAGE</button>
            <input ref={fileRef} type="file" accept="image/*" className="hidden" onChange={addImage} />
            <button onClick={()=>scrollBy(-1)} aria-label="left" className="w-12 h-12 rounded-full bg-[#2c3340] text-lg">◀</button>
            <button onClick={()=>scrollBy(1)} aria-label="right" className="w-12 h-12 rounded-full bg-[#1b2130] text-lg">▶</button>
          </div>
        </div>

        <div ref={scroller} className="flex gap-6 overflow-x-auto pb-4 scrollbar-hide">
          {images.map((src, idx)=>(
            <div key={idx} className="min-w-[260px] h-56 flex-shrink-0 rounded-2xl overflow-hidden shadow-md bg-gray-800">
              <img src={src} alt={`img-${idx}`} className="w-full h-full object-cover" />
            </div>
          ))}
        </div>
      </div>
    );
  }

  ReactDOM.createRoot(document.getElementById('root')).render(<App />);
  </script>
</body>
</html>
# SrikanthPortfolio.
Portfolio web app using React + Tailwind CSS
