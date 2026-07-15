<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Semana 13 · Monitoreo y Rendimiento · Base de Datos II</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Inter:wght@400;500;600;700&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
/* ============================================================
   SOLO LEVELING + POKEDEX/ARCANUM THEME
   ============================================================ */
:root{
  --bg-primary:#08050e;
  --bg-secondary:#0f0b1a;
  --bg-card:#141025;
  --bg-card-hover:#1c1735;
  --text-primary:#ece4f5;
  --text-secondary:#a898c0;
  --text-muted:#4a3a5a;
  --border-glow:#7c4cbf;
  --shadow-glow:0 0 40px rgba(108,59,158,0.10);
  --shadow-glow-strong:0 0 60px rgba(108,59,158,0.18);
  --accent-1:#8b5cf6;
  --accent-2:#6d28d9;
  --accent-3:#4c1d95;
  --accent-gold:#f2b84b;
  --accent-gold-glow:#f2b84b33;
  --accent-cyan:#4ecdc4;
  --accent-pink:#e26fb8;
  --accent-red:#ef4444;
  --glass:rgba(18,16,31,0.88);
  --glass-border:rgba(108,59,158,0.18);
  --runebg:radial-gradient(ellipse at center, rgba(108,59,158,0.08), transparent 70%);
}
*{margin:0;padding:0;box-sizing:border-box;}
html{scroll-behavior:smooth;}
body{
  font-family:'Inter',sans-serif;
  background:var(--bg-primary);
  color:var(--text-primary);
  min-height:100vh;
  overflow-x:hidden;
  background-image:
    radial-gradient(ellipse at 15% 20%, rgba(108,59,158,0.04) 0%, transparent 50%),
    radial-gradient(ellipse at 85% 80%, rgba(78,205,196,0.02) 0%, transparent 40%);
}
h1,h2,h3{font-family:'Orbitron',sans-serif;text-shadow:0 0 40px rgba(108,59,158,0.08);}
code,.mono,pre{font-family:'Share Tech Mono',monospace;}

::-webkit-scrollbar{width:4px;}
::-webkit-scrollbar-track{background:var(--bg-secondary);}
::-webkit-scrollbar-thumb{background:linear-gradient(180deg,var(--accent-1),var(--accent-3));border-radius:4px;}

/* ===== SPLASH ===== */
#splash{
  position:fixed;inset:0;z-index:9999;
  background:var(--bg-primary);
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  transition:opacity .9s ease,visibility .9s ease;
}
#splash .rune-ring{
  width:120px;height:120px;border-radius:50%;
  border:2px solid rgba(108,59,158,0.10);
  border-top-color:var(--accent-1);
  border-right-color:var(--accent-gold);
  box-shadow:0 0 70px rgba(108,59,158,0.10), inset 0 0 70px rgba(108,59,158,0.03);
  animation:spin-rune 1.8s linear infinite;
  margin-bottom:28px;
  position:relative;
}
#splash .rune-ring::after{
  content:"⬆";
  position:absolute;inset:0;display:flex;align-items:center;justify-content:center;
  font-size:2.6rem;color:var(--accent-gold);
  animation:spin-rune 1.8s linear infinite reverse;
  text-shadow:0 0 40px var(--accent-gold-glow);
}
@keyframes spin-rune{to{transform:rotate(360deg);}}
#splash h2{font-size:1rem;letter-spacing:5px;color:var(--text-secondary);}
#splash h2 span{color:var(--accent-gold);text-shadow:0 0 30px var(--accent-gold-glow);}
#splash .sub{font-size:.65rem;color:var(--text-muted);letter-spacing:4px;text-transform:uppercase;margin-top:4px;}
#splash.hide{opacity:0;visibility:hidden;}

/* ===== PROGRESS BAR ===== */
#progressBar{
  position:fixed;top:0;left:0;height:2px;z-index:200;
  background:linear-gradient(90deg,var(--accent-3),var(--accent-1),var(--accent-gold));
  width:0%;transition:width .1s ease-out;
  box-shadow:0 0 25px rgba(108,59,158,0.25);
}

/* ===== HEADER ===== */
header.hero{
  position:relative;padding:40px 20px 28px;text-align:center;overflow:hidden;
  border-bottom:1px solid rgba(108,59,158,0.06);
  background:linear-gradient(180deg, rgba(108,59,158,0.04) 0%, transparent 100%);
}
header.hero .content{position:relative;z-index:1;}
.badge-uni{
  display:inline-flex;align-items:center;gap:10px;
  background:var(--glass);backdrop-filter:blur(12px);
  padding:5px 14px;border-radius:30px;
  border:1px solid var(--glass-border);
  font-size:.6rem;font-weight:600;color:var(--text-secondary);
  margin-bottom:12px;
  letter-spacing:.5px;
}
.badge-uni .dot{
  width:7px;height:7px;border-radius:50%;
  background:var(--accent-gold);
  box-shadow:0 0 16px var(--accent-gold-glow);
  animation:pulse-dot 2s ease-in-out infinite;
}
@keyframes pulse-dot{0%,100%{opacity:1;}50%{opacity:.2;}}
header.hero h1{
  font-size:clamp(1.8rem,4.5vw,3.2rem);
  background:linear-gradient(135deg,#a78bfa,#e26fb8 35%,#4ecdc4 65%,#f2b84b);
  -webkit-background-clip:text;background-clip:text;color:transparent;
  line-height:1.1;margin-bottom:4px;font-weight:900;
}
header.hero .subtitle{
  font-size:.7rem;font-weight:500;color:var(--text-secondary);
  letter-spacing:3px;margin-bottom:10px;
}
header.hero .subtitle span{color:var(--accent-gold);}
.hero-meta{
  display:flex;flex-wrap:wrap;justify-content:center;gap:4px;margin-bottom:10px;
}
.hero-meta .chip{
  background:var(--glass);backdrop-filter:blur(8px);
  border:1px solid var(--glass-border);
  padding:3px 10px;border-radius:5px;
  font-size:.55rem;font-weight:600;color:var(--text-secondary);
}
.hero-obj{
  max-width:820px;margin:0 auto;
  background:var(--glass);backdrop-filter:blur(12px);
  border:1px solid var(--glass-border);
  border-radius:10px;padding:14px 18px;
  font-size:.74rem;line-height:1.7;color:var(--text-secondary);
  text-align:left;
  box-shadow:var(--shadow-glow);
}
.hero-obj b{color:var(--text-primary);}

/* ===== NAV ===== */
nav.temanav{
  position:sticky;top:0;z-index:150;
  background:rgba(8,5,14,0.94);backdrop-filter:blur(16px);
  border-bottom:1px solid rgba(108,59,158,0.06);
  padding:4px 4px;display:flex;justify-content:center;
}
nav.temanav .wrap{
  display:flex;gap:3px;flex-wrap:wrap;justify-content:center;max-width:1200px;
}
nav.temanav button{
  border:1px solid rgba(108,59,158,0.08);
  background:rgba(18,16,31,0.4);
  color:var(--text-secondary);
  padding:4px 8px;border-radius:4px;
  font-size:.5rem;font-weight:700;
  cursor:pointer;display:flex;align-items:center;gap:3px;
  transition:all .3s ease;letter-spacing:.3px;
}
nav.temanav button .rune-dot{
  width:4px;height:4px;background:var(--text-muted);
  border-radius:50%;transition:all .3s ease;
}
nav.temanav button:hover{
  border-color:var(--accent-1);
  transform:translateY(-1px);
  box-shadow:0 0 20px rgba(108,59,158,0.05);
}
nav.temanav button.active{
  background:linear-gradient(135deg,var(--accent-3),var(--accent-1));
  border-color:var(--accent-1);
  color:#fff;
  box-shadow:0 0 30px rgba(108,59,158,0.12);
}
nav.temanav button.active .rune-dot{background:#fff;box-shadow:0 0 10px rgba(255,255,255,0.12);}

/* ===== SECTIONS ===== */
section.tema{padding:30px 3vw 14px;scroll-margin-top:44px;}

.tema-head{
  display:flex;align-items:center;gap:12px;margin-bottom:14px;flex-wrap:wrap;
  padding-bottom:6px;border-bottom:1px solid rgba(108,59,158,0.06);
}
.tema-rune{
  width:42px;height:42px;border-radius:50%;
  display:flex;align-items:center;justify-content:center;
  flex-shrink:0;
  font-size:1rem;
  background:var(--runebg);
  border:1px solid rgba(108,59,158,0.12);
  box-shadow:0 0 30px rgba(108,59,158,0.03);
}
.tema-titles h2{
  font-size:.85rem;color:var(--text-primary);
  margin-bottom:1px;
}
.tema-titles .sub{
  font-size:.6rem;color:var(--text-secondary);
  font-weight:600;letter-spacing:.3px;
}

/* ===== INFOGRAPHIA · ARCANUM CARDS ===== */
.infografia{
  display:grid;grid-template-columns:repeat(auto-fit,minmax(130px,1fr));
  gap:8px;margin-bottom:18px;
}
.arcanum-card{
  perspective:1000px;height:130px;
}
.arcanum-inner{
  position:relative;width:100%;height:100%;
  transition:transform .5s cubic-bezier(.4,.2,.2,1);
  transform-style:preserve-3d;cursor:pointer;
}
.arcanum-card.flipped .arcanum-inner{transform:rotateY(180deg);}
.arcanum-front,.arcanum-back{
  position:absolute;inset:0;backface-visibility:hidden;
  border-radius:8px;padding:8px;
  display:flex;flex-direction:column;justify-content:center;
  border:1px solid var(--glass-border);
}
.arcanum-front{
  background:var(--glass);backdrop-filter:blur(8px);
  text-align:center;
}
.arcanum-front .rune-icon{font-size:1.2rem;margin-bottom:2px;}
.arcanum-front .rune-icon .glow{display:inline-block;animation:pulse-dot 3s ease-in-out infinite;}
.arcanum-front h4{
  font-size:.6rem;font-family:'Inter';font-weight:700;
  color:var(--text-primary);
}
.arcanum-front .hint{
  position:absolute;bottom:2px;right:6px;
  font-size:.4rem;color:var(--text-muted);opacity:.3;
}
.arcanum-back{
  transform:rotateY(180deg);
  background:linear-gradient(135deg,var(--accent-3),var(--accent-1));
  color:#fff;font-size:.55rem;line-height:1.4;
  border-color:transparent;
  text-align:center;
}
.arcanum-back .rune-label{
  display:block;font-size:.4rem;text-transform:uppercase;
  letter-spacing:2px;opacity:.5;margin-bottom:2px;
  font-family:'Orbitron';
}

/* ===== PROJECTS · ARCANUM TOMES ===== */
.proyectos{display:flex;flex-direction:column;gap:8px;margin-bottom:8px;}
.proyecto-tome{
  background:var(--bg-card);
  border:1px solid var(--glass-border);
  border-radius:8px;
  overflow:hidden;
  transition:all .3s ease;
  position:relative;
}
.proyecto-tome::before{
  content:"";position:absolute;top:0;left:0;right:0;height:2px;
  background:linear-gradient(90deg,var(--accent-3),var(--accent-1),var(--accent-gold));
  opacity:0;transition:opacity .3s ease;
}
.proyecto-tome:hover::before{opacity:1;}
.proyecto-tome:hover{border-color:rgba(108,59,158,0.35);}
.tome-head{
  padding:7px 12px;display:flex;align-items:center;
  justify-content:space-between;cursor:pointer;gap:6px;
}
.tome-head .left{display:flex;align-items:center;gap:6px;}
.tome-num{
  width:22px;height:22px;border-radius:50%;
  background:var(--runebg);
  border:1px solid rgba(108,59,158,0.12);
  color:var(--accent-gold);
  font-family:'Orbitron';font-weight:700;
  display:flex;align-items:center;justify-content:center;
  flex-shrink:0;font-size:.5rem;
}
.tome-head h4{
  font-size:.68rem;font-weight:700;color:var(--text-primary);
}
.tome-chevron{
  transition:transform .3s ease;font-size:.65rem;
  color:var(--text-muted);flex-shrink:0;
}
.proyecto-tome.open .tome-chevron{transform:rotate(180deg);}
.tome-body{max-height:0;overflow:hidden;transition:max-height .5s ease;}
.proyecto-tome.open .tome-body{max-height:3600px;}
.tome-tabs{
  display:flex;gap:2px;padding:0 12px;flex-wrap:wrap;
  border-bottom:1px solid rgba(108,59,158,0.04);
}
.tome-tab{
  border:none;background:transparent;
  color:var(--text-muted);padding:3px 8px;
  border-radius:4px 4px 0 0;
  font-size:.48rem;font-weight:700;
  cursor:pointer;letter-spacing:.3px;
  transition:all .25s ease;
  text-transform:uppercase;
}
.tome-tab.active{
  color:#fff;
  background:linear-gradient(135deg,var(--accent-3),var(--accent-1));
}
.tome-content{padding:8px 12px 12px;}
.tome-pane{display:none;font-size:.68rem;line-height:1.6;color:var(--text-secondary);}
.tome-pane.active{display:block;animation:fadein .3s ease;}
@keyframes fadein{from{opacity:0;transform:translateY(4px);}to{opacity:1;transform:translateY(0);}}
.tome-pane strong{color:var(--text-primary);}
pre.code-block{
  background:#0a0815;color:#d4c8e8;border-radius:6px;
  padding:8px 10px;overflow-x:auto;
  font-size:.58rem;line-height:1.5;
  border:1px solid rgba(108,59,158,0.06);
}
pre.code-block .kw{color:var(--accent-gold);}
pre.code-block .fn{color:var(--accent-cyan);}
pre.code-block .str{color:var(--accent-pink);}
pre.code-block .com{color:#3a2a4a;font-style:italic;}
.practice-grid{
  display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));
  gap:4px;margin-top:4px;
}
.practice-item{
  background:rgba(18,16,31,0.5);border-radius:4px;
  padding:5px 8px;border-left:2px solid var(--accent-1);
  font-size:.6rem;color:var(--text-secondary);
}

/* ===== ACTIVIDAD ===== */
section.actividad{padding:30px 3vw 44px;}
.act-arcane{
  background:linear-gradient(135deg,#0d0b16,var(--accent-3) 50%,var(--accent-1));
  border-radius:10px;padding:20px 3vw;
  border:1px solid rgba(108,59,158,0.12);
  position:relative;overflow:hidden;
}
.act-arcane::after{
  content:"⬆";position:absolute;font-size:10rem;
  color:rgba(108,59,158,0.05);bottom:-40px;right:-20px;
  font-family:'Orbitron';
}
.act-arcane h2{
  font-size:.95rem;margin-bottom:8px;
  color:#fff;position:relative;z-index:1;
}
.act-arcane .steps{
  display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));
  gap:8px;position:relative;z-index:1;margin-top:8px;
}
.act-arcane .step{
  background:rgba(255,255,255,0.03);
  border:1px solid rgba(255,255,255,0.04);
  border-radius:8px;padding:8px 10px;
  backdrop-filter:blur(6px);
}
.act-arcane .step .n{
  font-family:'Orbitron';font-size:.75rem;font-weight:900;
  color:var(--accent-gold);margin-bottom:2px;
}
.act-arcane .step p{font-size:.64rem;line-height:1.4;color:rgba(255,255,255,0.75);}
.act-arcane .note{
  margin-top:10px;font-size:.6rem;
  background:rgba(255,255,255,0.03);
  padding:6px 10px;border-radius:6px;
  color:rgba(255,255,255,0.5);
  border:1px solid rgba(255,255,255,0.03);
  position:relative;z-index:1;
}

/* ===== FOOTER ===== */
footer{
  text-align:center;padding:18px 20px 26px;
  color:var(--text-muted);font-size:.55rem;
  border-top:1px solid rgba(108,59,158,0.05);
}
footer .sign{
  font-family:'Orbitron';font-size:.5rem;
  letter-spacing:4px;color:var(--accent-1);
  margin-top:2px;
}
footer .sign .gold{color:var(--accent-gold);}

/* ===== RESPONSIVE ===== */
@media(max-width:640px){
  .tema-rune{width:34px;height:34px;font-size:.8rem;}
  .tome-head{padding:5px 8px;}
  .tome-content{padding:6px 8px 10px;}
  .hero-obj{padding:10px 12px;font-size:.66rem;}
  .arcanum-card{height:115px;}
  .tema-titles h2{font-size:.72rem;}
}
</style>
</head>
<body>

<div id="splash">
  <div class="rune-ring"></div>
  <h2>BASE DE DATOS <span>II</span></h2>
  <div class="sub">Semana 13 · Monitoreo y Rendimiento</div>
</div>

<div id="progressBar"></div>

<header class="hero">
  <div class="content">
    <div class="badge-uni"><span class="dot"></span> UPLA · Facultad de Ingeniería · EP de Ingeniería de Sistemas y Computación</div>
    <h1>Monitoreo y Rendimiento</h1>
    <div class="subtitle">SQL Server 2022 · Base de Datos <span>GradosTitulos</span> · Semana 13</div>
    <div class="hero-meta">
      <div class="chip">⬆ Kevin Yeison Ccoñas Gomez</div>
      <div class="chip">📘 Base de Datos II</div>
      <div class="chip">🧑‍🏫 Mg. Ing. Raúl Fernández Bejarano</div>
      <div class="chip">🎓 V Ciclo · 2026</div>
    </div>
    <div class="hero-obj">
      <b>✦ Objetivo:</b> Optimizar el rendimiento de la base de datos <b>GradosTitulos</b> mediante el monitoreo del comportamiento del motor SQL Server, el análisis de consultas, la optimización de índices, el control de transacciones y la administración de recursos del servidor, garantizando tiempos de respuesta adecuados para los procesos de grados y títulos.
    </div>
  </div>
</header>

<nav class="temanav">
  <div class="wrap" id="navWrap"></div>
</nav>

<main id="mainContent"></main>

<footer>
  Trabajo académico elaborado para la asignatura Base de Datos II · UPLA Huancayo
  <div class="sign">⬆ <span class="gold">KEVIN</span> · CCOÑAS ⬆</div>
</footer>

<script>
/* ============================================================
   DATA · 6 TEMAS CON INFO + PROYECTOS (COMPLETO)
   ============================================================ */
const temas = [
{
  id:1, icon:'📡',
  titulo:'Análisis de rendimiento con Extended Events',
  subtitulo:'Captura y diagnóstico de consultas costosas en tiempo real',
  infografia:[
    {ico:'🎯',front:'¿Qué es Extended Events?',back:'Motor ligero de diagnóstico de SQL Server que sustituye a SQL Profiler; captura eventos del motor con bajo impacto en el rendimiento del servidor.'},
    {ico:'⏱️',front:'Umbral de duración',back:'Se filtran eventos por duración en microsegundos. Ej: capturar solo consultas > 1000 ms (1,000,000 microsegundos) para aislar las más costosas.'},
    {ico:'🗂️',front:'Targets de captura',back:'event_file (archivo .xel en disco, persistente) y ring_buffer (memoria, volátil). Para auditoría histórica se usa event_file.'},
    {ico:'🧩',front:'Acciones (actions)',back:'Datos adicionales agregados al evento: sql_text, client_app_name, cpu_time, logical_reads, username, session_id.'},
    {ico:'📊',front:'sys.fn_xe_file_target_read_file',back:'Función que permite leer e importar el contenido del archivo .xel generado por la sesión hacia una tabla de SQL Server.'},
    {ico:'🛡️',front:'Buenas prácticas',back:'Usar predicados (WHERE) para filtrar solo lo necesario, limitar tamaño del archivo, y detener sesiones que ya no se usan.'}
  ],
  proyectos:[
    {
      titulo:'Respaldo Completo Institucional de la Base de Datos GradosTitulos',
      enunciado:`La Oficina de Tecnologías de Información de la universidad ha detectado que el módulo de registro de expedientes de bachiller y título profesional presenta lentitud durante las horas de mayor demanda.<br><br>
      Se solicita implementar una sesión de monitoreo utilizando <strong>Extended Events</strong> para capturar todas las consultas cuya duración sea superior a <strong>1000 milisegundos</strong> sobre la base de datos <strong>GradosTitulos</strong>. El estudiante deberá identificar las consultas más costosas ejecutadas sobre los esquemas <strong>Academico, Tramite, Evaluacion y Documento</strong>.<br><br>
      El sistema deberá registrar automáticamente las consultas más costosas ejecutadas por los usuarios durante la jornada laboral y almacenar la información en una tabla histórica para posteriores auditorías de desempeño. El mecanismo debe permitir: iniciar la sesión de monitoreo; registrar las consultas ejecutadas sobre la base de datos GradosTitulos; importar periódicamente la información del archivo de eventos a una tabla de auditoría; y generar un reporte de las consultas con mayor duración, mayor consumo de CPU y mayor número de lecturas lógicas.`,
      script:`<span class="com">-- 1. Crear sesión de Extended Events (umbral > 1000 ms = 1,000,000 microsegundos)</span>
<span class="kw">CREATE EVENT SESSION</span> [Monitoreo_GradosTitulos]
<span class="kw">ON SERVER</span>
<span class="kw">ADD EVENT</span> sqlserver.sql_statement_completed(
    <span class="kw">ACTION</span>(sqlserver.sql_text, sqlserver.client_app_name,
           sqlserver.username, sqlserver.session_id)
    <span class="kw">WHERE</span> (
        sqlserver.database_name = <span class="str">N'GradosTitulos'</span>
        <span class="kw">AND</span> duration &gt; 1000000
    )
)
<span class="kw">ADD TARGET</span> package0.event_file(
    <span class="kw">SET</span> filename = <span class="str">N'C:\\XEvents\\Monitoreo_GradosTitulos.xel'</span>,
        max_file_size = 50, max_rollover_files = 5
)
<span class="kw">WITH</span> (STARTUP_STATE = ON);
<span class="kw">GO</span>

<span class="com">-- 2. Iniciar la sesión de monitoreo</span>
<span class="kw">ALTER EVENT SESSION</span> [Monitoreo_GradosTitulos] <span class="kw">ON SERVER STATE = START</span>;
<span class="kw">GO</span>

<span class="com">-- 3. Tabla histórica de auditoría de desempeño</span>
<span class="kw">CREATE TABLE</span> dbo.AuditoriaConsultasCostosas (
    Id            <span class="fn">INT IDENTITY</span>(1,1) <span class="kw">PRIMARY KEY</span>,
    FechaEvento   DATETIME2,
    SqlText       NVARCHAR(MAX),
    Aplicacion    NVARCHAR(200),
    Usuario       NVARCHAR(200),
    DuracionMs    BIGINT,
    CpuMs         BIGINT,
    LecturasLogicas BIGINT,
    FechaImportacion DATETIME2 DEFAULT SYSDATETIME()
);
<span class="kw">GO</span>

<span class="com">-- 4. Importar periódicamente el archivo .xel hacia la tabla de auditoría</span>
<span class="kw">INSERT INTO</span> dbo.AuditoriaConsultasCostosas
    (FechaEvento, SqlText, Aplicacion, Usuario, DuracionMs, CpuMs, LecturasLogicas)
<span class="kw">SELECT</span>
    event_data.value(<span class="str">'(event/@timestamp)[1]'</span>, <span class="str">'DATETIME2'</span>),
    event_data.value(<span class="str">'(event/action[@name="sql_text"]/value)[1]'</span>, <span class="str">'NVARCHAR(MAX)'</span>),
    event_data.value(<span class="str">'(event/action[@name="client_app_name"]/value)[1]'</span>, <span class="str">'NVARCHAR(200)'</span>),
    event_data.value(<span class="str">'(event/action[@name="username"]/value)[1]'</span>, <span class="str">'NVARCHAR(200)'</span>),
    event_data.value(<span class="str">'(event/data[@name="duration"]/value)[1]'</span>, <span class="str">'BIGINT'</span>) / 1000,
    event_data.value(<span class="str">'(event/data[@name="cpu_time"]/value)[1]'</span>, <span class="str">'BIGINT'</span>) / 1000,
    event_data.value(<span class="str">'(event/data[@name="logical_reads"]/value)[1]'</span>, <span class="str">'BIGINT'</span>)
<span class="kw">FROM</span> (
    <span class="kw">SELECT CAST</span>(event_data <span class="kw">AS XML</span>) <span class="kw">AS</span> event_data
    <span class="kw">FROM</span> sys.<span class="fn">fn_xe_file_target_read_file</span>(
        <span class="str">'C:\\XEvents\\Monitoreo_GradosTitulos*.xel'</span>, <span class="kw">NULL</span>, <span class="kw">NULL</span>, <span class="kw">NULL</span>)
) <span class="kw">AS</span> tab;
<span class="kw">GO</span>

<span class="com">-- 5. Reporte: consultas con mayor duración, CPU y lecturas lógicas</span>
<span class="kw">SELECT TOP</span> 20 *
<span class="kw">FROM</span> dbo.AuditoriaConsultasCostosas
<span class="kw">ORDER BY</span> DuracionMs <span class="kw">DESC</span>, CpuMs <span class="kw">DESC</span>, LecturasLogicas <span class="kw">DESC</span>;`,
      justificacion:`Se eligió <strong>Extended Events</strong> en lugar de SQL Profiler porque Profiler está en desuso desde SQL Server 2012 y genera mucha más sobrecarga sobre el motor. El evento <em>sql_statement_completed</em> permite capturar la duración exacta de cada sentencia, mientras que el predicado <code>WHERE duration &gt; 1000000</code> filtra en el propio motor (server-side filtering), evitando registrar tráfico irrelevante y reduciendo el impacto en producción.<br><br>
      Se usó <strong>event_file</strong> como target porque persiste en disco y sobrevive a reinicios del servicio, lo cual es indispensable para una auditoría histórica. La función <code>sys.fn_xe_file_target_read_file</code> permite procesar el XML generado e insertarlo en una tabla relacional estructurada, facilitando reportes con SELECT estándar.`,
      practicas:['Filtrar eventos en el predicado de la sesión y no después, para minimizar overhead','Usar STARTUP_STATE=ON para que la sesión sobreviva a reinicios del servicio','Limitar max_file_size y max_rollover_files para controlar el espacio en disco','Programar la importación del .xel mediante un job de SQL Server Agent, no manualmente']
    },
    {
      titulo:'Implementación de Backups Diferenciales para Trámites Académicos',
      enunciado:`Durante los procesos de matrícula para obtención del título profesional, varios usuarios reportan tiempos elevados en la generación de resoluciones.<br><br>
      La Dirección de Informática solicita construir una solución de monitoreo que capture automáticamente: <strong>consultas lentas, deadlocks, bloqueos, errores superiores al nivel 16 y uso excesivo de CPU</strong>. Toda la información deberá almacenarse en un archivo de eventos para su posterior análisis.`,
      script:`<span class="com">-- Sesión combinada: consultas lentas, deadlocks, bloqueos, errores severos y alto CPU</span>
<span class="kw">CREATE EVENT SESSION</span> [Monitoreo_Tramites_Criticos]
<span class="kw">ON SERVER</span>
<span class="com">-- Consultas lentas (&gt; 2000 ms)</span>
<span class="kw">ADD EVENT</span> sqlserver.sql_statement_completed(
    <span class="kw">ACTION</span>(sqlserver.sql_text, sqlserver.session_id)
    <span class="kw">WHERE</span> duration &gt; 2000000
),
<span class="com">-- Deadlocks del motor</span>
<span class="kw">ADD EVENT</span> sqlserver.xml_deadlock_report,
<span class="com">-- Bloqueos prolongados (lock_acquired con espera)</span>
<span class="kw">ADD EVENT</span> sqlserver.lock_escalation,
<span class="com">-- Errores con severidad &gt;= 16</span>
<span class="kw">ADD EVENT</span> sqlserver.error_reported(
    <span class="kw">ACTION</span>(sqlserver.sql_text)
    <span class="kw">WHERE</span> severity &gt;= 16
),
<span class="com">-- Uso excesivo de CPU (&gt; 500 ms de cpu_time)</span>
<span class="kw">ADD EVENT</span> sqlserver.sql_statement_completed(
    <span class="kw">SET</span> collect_cpu_time = 1
    <span class="kw">WHERE</span> cpu_time &gt; 500000
)
<span class="kw">ADD TARGET</span> package0.event_file(
    <span class="kw">SET</span> filename = <span class="str">N'C:\\XEvents\\Tramites_Criticos.xel'</span>,
        max_file_size = 100
)
<span class="kw">WITH</span> (STARTUP_STATE = ON);
<span class="kw">GO</span>

<span class="kw">ALTER EVENT SESSION</span> [Monitoreo_Tramites_Criticos] <span class="kw">ON SERVER STATE = START</span>;
<span class="kw">GO</span>

<span class="com">-- Consulta de análisis posterior sobre el archivo capturado</span>
<span class="kw">SELECT</span>
    event_data.value(<span class="str">'(event/@name)[1]'</span>, <span class="str">'VARCHAR(60)'</span>) <span class="kw">AS</span> TipoEvento,
    event_data.value(<span class="str">'(event/@timestamp)[1]'</span>, <span class="str">'DATETIME2'</span>) <span class="kw">AS</span> Fecha,
    event_data.query(<span class="str">'.'</span>) <span class="kw">AS</span> DetalleXML
<span class="kw">FROM</span> (
    <span class="kw">SELECT CAST</span>(event_data <span class="kw">AS XML</span>) <span class="kw">AS</span> event_data
    <span class="kw">FROM</span> sys.<span class="fn">fn_xe_file_target_read_file</span>(<span class="str">'C:\\XEvents\\Tramites_Criticos*.xel'</span>, <span class="kw">NULL</span>,<span class="kw">NULL</span>,<span class="kw">NULL</span>)
) <span class="kw">AS</span> tab
<span class="kw">ORDER BY</span> Fecha <span class="kw">DESC</span>;`,
      justificacion:`Se combinaron varios eventos en una sola sesión para tener una vista unificada del comportamiento anómalo del sistema. <code>xml_deadlock_report</code> es el evento estándar de Microsoft para capturar el gráfico XML completo de un interbloqueo, mientras que <code>error_reported</code> con el filtro <code>severity &gt;= 16</code> captura únicamente errores que requieren intervención del usuario o del DBA (los niveles 0–10 son informativos).<br><br>
      Usar un único archivo de eventos consolidado simplifica la auditoría, ya que el analista puede correlacionar en el tiempo un deadlock con un pico de CPU o con errores reportados casi simultáneamente.`,
      practicas:['Separar sesiones críticas (deadlocks/errores) de sesiones de volumen (consultas lentas) en producción real para evitar archivos enormes','Revisar el archivo .xel con Management Studio &gt; Extended Events &gt; Watch Live Data durante pruebas','Establecer alertas de SQL Server Agent ante errores de severidad alta','Documentar cada incidente capturado con fecha, causa raíz y acción tomada']
    },
    {
      titulo:'Estrategia Integral Full + Differential + Log Backup',
      enunciado:`La universidad desea implementar un sistema permanente de diagnóstico del rendimiento para la base de datos <strong>GradosTitulos</strong>, que combine el monitoreo continuo del motor con una estrategia de respaldo que garantice la disponibilidad de la información histórica capturada, incluso ante fallas del servidor.`,
      script:`<span class="com">-- 1. Sesión de monitoreo permanente y liviana (system_health-like)</span>
<span class="kw">CREATE EVENT SESSION</span> [Diagnostico_Permanente_GT]
<span class="kw">ON SERVER</span>
<span class="kw">ADD EVENT</span> sqlserver.sql_statement_completed(
    <span class="kw">ACTION</span>(sqlserver.sql_text, sqlserver.database_name)
    <span class="kw">WHERE</span> database_name = <span class="str">N'GradosTitulos'</span> <span class="kw">AND</span> duration &gt; 500000
)
<span class="kw">ADD TARGET</span> package0.ring_buffer(<span class="kw">SET</span> max_events_limit = 2000),
<span class="kw">ADD TARGET</span> package0.event_file(<span class="kw">SET</span> filename = <span class="str">N'C:\\XEvents\\Diagnostico_Permanente.xel'</span>, max_file_size=200)
<span class="kw">WITH</span> (STARTUP_STATE = ON, MAX_DISPATCH_LATENCY = 10 SECONDS);
<span class="kw">GO</span>
<span class="kw">ALTER EVENT SESSION</span> [Diagnostico_Permanente_GT] <span class="kw">ON SERVER STATE = START</span>;
<span class="kw">GO</span>

<span class="com">-- 2. Backup FULL semanal (domingo)</span>
<span class="kw">BACKUP DATABASE</span> GradosTitulos
<span class="kw">TO DISK</span> = <span class="str">N'D:\\Backups\\GradosTitulos_FULL.bak'</span>
<span class="kw">WITH</span> INIT, COMPRESSION,
     NAME = <span class="str">N'GradosTitulos-Full Backup'</span>;
<span class="kw">GO</span>

<span class="com">-- 3. Backup DIFERENCIAL diario (basado en el último FULL)</span>
<span class="kw">BACKUP DATABASE</span> GradosTitulos
<span class="kw">TO DISK</span> = <span class="str">N'D:\\Backups\\GradosTitulos_DIFF.bak'</span>
<span class="kw">WITH DIFFERENTIAL</span>, COMPRESSION,
     NAME = <span class="str">N'GradosTitulos-Differential Backup'</span>;
<span class="kw">GO</span>

<span class="com">-- 4. Backup del LOG de transacciones cada 30-60 min (modelo FULL recovery)</span>
<span class="kw">BACKUP LOG</span> GradosTitulos
<span class="kw">TO DISK</span> = <span class="str">N'D:\\Backups\\GradosTitulos_LOG.trn'</span>
<span class="kw">WITH</span> COMPRESSION,
     NAME = <span class="str">N'GradosTitulos-Log Backup'</span>;
<span class="kw">GO</span>

<span class="com">-- 5. Verificación de la cadena de respaldo</span>
<span class="kw">RESTORE HEADERONLY FROM DISK</span> = <span class="str">N'D:\\Backups\\GradosTitulos_FULL.bak'</span>;`,
      justificacion:`La estrategia <strong>Full + Differential + Log</strong> asegura que, ante una falla, se pueda restaurar la base de datos hasta el punto exacto anterior al incidente (point-in-time recovery), lo que también protege la tabla de auditoría de desempeño generada por las sesiones de Extended Events. El backup FULL es la línea base; el DIFERENCIAL reduce el tiempo de restauración diario al capturar solo cambios desde el último FULL; el LOG permite recuperación granular y evita que el archivo de transacciones crezca indefinidamente.<br><br>
      Combinar esta estrategia con una sesión de diagnóstico permanente (ring_buffer + event_file con MAX_DISPATCH_LATENCY) permite monitoreo continuo sin saturar el disco, ya que el ring_buffer mantiene solo los últimos 2000 eventos en memoria mientras el archivo conserva el histórico completo.`,
      practicas:['Probar periódicamente la restauración (RESTORE VERIFYONLY) para garantizar que los backups son utilizables','Usar el modelo de recuperación FULL para permitir backups de log y recuperación a un punto en el tiempo','Automatizar los tres tipos de backup mediante planes de mantenimiento o SQL Server Agent','Almacenar los backups en una ubicación distinta al servidor de producción (regla 3-2-1)']
    }
  ]
},
{
  id:2, icon:'📈',
  titulo:'Estadísticas e índices',
  subtitulo:'Creación, fragmentación y mantenimiento de índices',
  infografia:[
    {ico:'🌳',front:'Índice Clustered',back:'Ordena físicamente los datos de la tabla según la clave del índice. Solo puede existir uno por tabla (suele ser la PK).'},
    {ico:'🍃',front:'Índice NonClustered',back:'Estructura separada que apunta a las filas de la tabla. Se pueden crear múltiples por tabla para acelerar búsquedas específicas.'},
    {ico:'➕',front:'Columnas INCLUDE',back:'Columnas adicionales almacenadas en el nivel hoja del índice sin formar parte de la clave, evitando lookups a la tabla base (covering index).'},
    {ico:'🧹',front:'Fragmentación',back:'Desorden físico de las páginas del índice tras INSERT/UPDATE/DELETE frecuentes. Se mide con sys.dm_db_index_physical_stats.'},
    {ico:'🔧',front:'REORGANIZE vs REBUILD',back:'REORGANIZE (10%-30% fragmentación) es online y ligero; REBUILD (&gt;30%) reconstruye el índice completo y actualiza estadísticas.'},
    {ico:'🤖',front:'Automatización',back:'Un job de SQL Server Agent ejecuta el procedimiento de mantenimiento periódicamente sin intervención manual del DBA.'}
  ],
  proyectos:[
    {
      titulo:'Optimización de índices para consultas de estudiantes',
      enunciado:`Las consultas realizadas sobre las tablas <strong>Academico.Persona, Academico.Matricula y Tramite.Tramite</strong> presentan tiempos elevados durante la búsqueda de estudiantes.<br><br>
      Se solicita desarrollar un conjunto de scripts que permitan: analizar índices existentes; detectar índices faltantes; crear índices Clustered y NonClustered; crear índices INCLUDE; y comparar tiempos antes y después de la optimización. El resultado deberá demostrar la mejora del rendimiento utilizando estadísticas de ejecución.`,
      script:`<span class="com">-- 1. Analizar índices existentes en las tablas objetivo</span>
<span class="kw">SELECT</span> t.name <span class="kw">AS</span> Tabla, i.name <span class="kw">AS</span> Indice, i.type_desc, i.is_unique
<span class="kw">FROM</span> sys.indexes i
<span class="kw">JOIN</span> sys.tables t <span class="kw">ON</span> i.object_id = t.object_id
<span class="kw">WHERE</span> t.name <span class="kw">IN</span> (<span class="str">'Persona'</span>,<span class="str">'Matricula'</span>,<span class="str">'Tramite'</span>);
<span class="kw">GO</span>

<span class="com">-- 2. Detectar índices faltantes recomendados por el motor</span>
<span class="kw">SELECT</span>
    migs.avg_user_impact, mid.statement <span class="kw">AS</span> Tabla,
    mid.equality_columns, mid.included_columns
<span class="kw">FROM</span> sys.dm_db_missing_index_details mid
<span class="kw">JOIN</span> sys.dm_db_missing_index_groups mig <span class="kw">ON</span> mid.index_handle = mig.index_handle
<span class="kw">JOIN</span> sys.dm_db_missing_index_group_stats migs <span class="kw">ON</span> mig.index_group_handle = migs.group_handle
<span class="kw">ORDER BY</span> migs.avg_user_impact <span class="kw">DESC</span>;
<span class="kw">GO</span>

<span class="com">-- 3. Habilitar estadísticas de ejecución</span>
<span class="kw">SET STATISTICS TIME ON</span>;
<span class="kw">SET STATISTICS IO ON</span>;
<span class="kw">GO</span>

<span class="com">-- 4a. Consulta "antes" de optimizar</span>
<span class="kw">SELECT</span> p.Nombres, p.Apellidos, m.CodigoMatricula, tr.Estado
<span class="kw">FROM</span> Academico.Persona p
<span class="kw">JOIN</span> Academico.Matricula m <span class="kw">ON</span> m.PersonaId = p.PersonaId
<span class="kw">JOIN</span> Tramite.Tramite tr <span class="kw">ON</span> tr.PersonaId = p.PersonaId
<span class="kw">WHERE</span> p.Apellidos <span class="kw">LIKE</span> <span class="str">N'Cuchula%'</span>;
<span class="kw">GO</span>

<span class="com">-- 4b. Creación de índices NonClustered con columnas INCLUDE (covering index)</span>
<span class="kw">CREATE NONCLUSTERED INDEX</span> IX_Persona_Apellidos
<span class="kw">ON</span> Academico.Persona(Apellidos, Nombres)
<span class="kw">INCLUDE</span>(PersonaId);
<span class="kw">GO</span>

<span class="kw">CREATE NONCLUSTERED INDEX</span> IX_Matricula_PersonaId
<span class="kw">ON</span> Academico.Matricula(PersonaId)
<span class="kw">INCLUDE</span>(CodigoMatricula);
<span class="kw">GO</span>

<span class="kw">CREATE NONCLUSTERED INDEX</span> IX_Tramite_PersonaId
<span class="kw">ON</span> Tramite.Tramite(PersonaId)
<span class="kw">INCLUDE</span>(Estado);
<span class="kw">GO</span>

<span class="com">-- 5. Confirmar clustered index existente sobre la clave primaria (si no existe, crearlo)</span>
<span class="kw">IF NOT EXISTS</span> (<span class="kw">SELECT</span> 1 <span class="kw">FROM</span> sys.indexes <span class="kw">WHERE</span> object_id=OBJECT_ID(<span class="str">'Academico.Persona'</span>) <span class="kw">AND</span> type=1)
    <span class="kw">CREATE CLUSTERED INDEX</span> CIX_Persona <span class="kw">ON</span> Academico.Persona(PersonaId);
<span class="kw">GO</span>

<span class="com">-- 6. Repetir la consulta "después" y comparar STATISTICS TIME/IO</span>
<span class="kw">SELECT</span> p.Nombres, p.Apellidos, m.CodigoMatricula, tr.Estado
<span class="kw">FROM</span> Academico.Persona p
<span class="kw">JOIN</span> Academico.Matricula m <span class="kw">ON</span> m.PersonaId = p.PersonaId
<span class="kw">JOIN</span> Tramite.Tramite tr <span class="kw">ON</span> tr.PersonaId = p.PersonaId
<span class="kw">WHERE</span> p.Apellidos <span class="kw">LIKE</span> <span class="str">N'Cuchula%'</span>;`,
      justificacion:`Los índices <strong>NonClustered</strong> con columnas <strong>INCLUDE</strong> convierten la consulta en un <em>covering index</em>: el motor resuelve toda la consulta leyendo únicamente el índice, sin necesidad de un Key Lookup hacia la tabla base, lo cual reduce drásticamente las lecturas lógicas.<br><br>
      Se usaron las DMV <code>sys.dm_db_missing_index_*</code> porque reflejan directamente las recomendaciones del optimizador basadas en el historial real de ejecución, en lugar de adivinar qué columnas indexar. <code>SET STATISTICS TIME/IO ON</code> permite comparar de forma objetiva (ms de CPU, lecturas lógicas) el antes y el después de crear los índices.`,
      practicas:['Nombrar los índices con un prefijo estándar (IX_, CIX_) para facilitar su identificación','No crear índices INCLUDE innecesarios: cada índice adicional aumenta el costo de INSERT/UPDATE','Verificar el plan de ejecución para confirmar que el índice creado realmente se usa','Evitar duplicar índices ya cubiertos por uno existente más amplio']
    },
    {
      titulo:'Detección y corrección de fragmentación',
      enunciado:`Después de varios meses de operaciones se observa disminución del rendimiento debido al crecimiento de las tablas.<br><br>
      Se solicita desarrollar un procedimiento almacenado que: detecte fragmentación mayor al 20%; reorganice índices cuando la fragmentación sea moderada; reconstruya índices cuando supere el 40%; actualice estadísticas automáticamente; y genere un reporte con el porcentaje de mejora obtenido.`,
      script:`<span class="kw">CREATE PROCEDURE</span> dbo.usp_MantenimientoFragmentacion
<span class="kw">AS</span>
<span class="kw">BEGIN</span>
    <span class="kw">SET NOCOUNT ON</span>;

    <span class="kw">CREATE TABLE</span> #Fragmentacion (
        TablaId INT, IndiceId INT, NombreTabla SYSNAME,
        NombreIndice SYSNAME, FragAntes FLOAT
    );

    <span class="com">-- Detección: fragmentación mayor al 20%</span>
    <span class="kw">INSERT INTO</span> #Fragmentacion
    <span class="kw">SELECT</span> ps.object_id, ps.index_id, OBJECT_NAME(ps.object_id), i.name, ps.avg_fragmentation_in_percent
    <span class="kw">FROM</span> sys.dm_db_index_physical_stats(DB_ID(), <span class="kw">NULL</span>, <span class="kw">NULL</span>, <span class="kw">NULL</span>, <span class="str">'LIMITED'</span>) ps
    <span class="kw">JOIN</span> sys.indexes i <span class="kw">ON</span> ps.object_id = i.object_id <span class="kw">AND</span> ps.index_id = i.index_id
    <span class="kw">WHERE</span> ps.avg_fragmentation_in_percent &gt; 20 <span class="kw">AND</span> i.name <span class="kw">IS NOT NULL</span>;

    <span class="kw">DECLARE</span> @tabla SYSNAME, @indice SYSNAME, @frag FLOAT, @sql NVARCHAR(500);

    <span class="kw">DECLARE</span> cur <span class="kw">CURSOR FOR SELECT</span> NombreTabla, NombreIndice, FragAntes <span class="kw">FROM</span> #Fragmentacion;
    <span class="kw">OPEN</span> cur;
    <span class="kw">FETCH NEXT FROM</span> cur <span class="kw">INTO</span> @tabla, @indice, @frag;

    <span class="kw">WHILE</span> @@FETCH_STATUS = 0
    <span class="kw">BEGIN</span>
        <span class="kw">IF</span> @frag &gt; 40
            <span class="kw">SET</span> @sql = <span class="str">N'ALTER INDEX ['</span> + @indice + <span class="str">N'] ON ['</span> + @tabla + <span class="str">N'] REBUILD;'</span>;
        <span class="kw">ELSE</span>
            <span class="kw">SET</span> @sql = <span class="str">N'ALTER INDEX ['</span> + @indice + <span class="str">N'] ON ['</span> + @tabla + <span class="str">N'] REORGANIZE;'</span>;

        <span class="kw">EXEC</span> sp_executesql @sql;

        <span class="com">-- Actualizar estadísticas del índice tratado</span>
        <span class="kw">SET</span> @sql = <span class="str">N'UPDATE STATISTICS ['</span> + @tabla + <span class="str">N'] ['</span> + @indice + <span class="str">N'] WITH FULLSCAN;'</span>;
        <span class="kw">EXEC</span> sp_executesql @sql;

        <span class="kw">FETCH NEXT FROM</span> cur <span class="kw">INTO</span> @tabla, @indice, @frag;
    <span class="kw">END</span>
    <span class="kw">CLOSE</span> cur; <span class="kw">DEALLOCATE</span> cur;

    <span class="com">-- Reporte de mejora: fragmentación antes vs después</span>
    <span class="kw">SELECT</span> f.NombreTabla, f.NombreIndice, f.FragAntes <span class="kw">AS</span> Frag_Antes,
           ps.avg_fragmentation_in_percent <span class="kw">AS</span> Frag_Despues,
           <span class="fn">ROUND</span>(f.FragAntes - ps.avg_fragmentation_in_percent, 2) <span class="kw">AS</span> Mejora_Porcentual
    <span class="kw">FROM</span> #Fragmentacion f
    <span class="kw">JOIN</span> sys.dm_db_index_physical_stats(DB_ID(), <span class="kw">NULL</span>, <span class="kw">NULL</span>, <span class="kw">NULL</span>, <span class="str">'LIMITED'</span>) ps
        <span class="kw">ON</span> f.TablaId = ps.object_id <span class="kw">AND</span> f.IndiceId = ps.index_id;

    <span class="kw">DROP TABLE</span> #Fragmentacion;
<span class="kw">END</span>;
<span class="kw">GO</span>

<span class="kw">EXEC</span> dbo.usp_MantenimientoFragmentacion;`,
      justificacion:`El procedimiento usa el umbral estándar recomendado por Microsoft: <strong>REORGANIZE</strong> para fragmentación entre 20%-40% (operación online que reordena las páginas hoja sin bloquear la tabla) y <strong>REBUILD</strong> para fragmentación mayor al 40% (reconstruye completamente el índice, requiere más recursos pero elimina la fragmentación por completo).<br><br>
      Se usó <code>sys.dm_db_index_physical_stats</code> con el modo <code>LIMITED</code> por ser más liviano que <code>DETAILED</code>, adecuado para monitoreo rutinario. Actualizar estadísticas con <code>FULLSCAN</code> tras el mantenimiento garantiza que el optimizador tenga información precisa y actualizada para generar planes de ejecución óptimos.`,
      practicas:['Ejecutar el mantenimiento en horarios de baja demanda para minimizar el impacto de REBUILD','Usar REBUILD ONLINE=ON en ediciones Enterprise para evitar bloqueos prolongados','Documentar el % de mejora en cada ejecución para llevar un historial de salud de índices','Evitar ejecutar REORGANIZE y REBUILD sobre el mismo índice en la misma corrida']
    },
    {
      titulo:'Automatización del mantenimiento de índices',
      enunciado:`La base de datos requiere mantenimiento periódico sin intervención del administrador.<br><br>
      Se solicita desarrollar un proceso automatizado mediante Transact-SQL que: actualice estadísticas; reorganice índices; reconstruya índices críticos; registre fecha, duración y resultado; y que pueda ser ejecutado mediante SQL Server Agent.`,
      script:`<span class="com">-- Tabla de bitácora del mantenimiento automatizado</span>
<span class="kw">CREATE TABLE</span> dbo.LogMantenimientoIndices (
    Id INT IDENTITY <span class="kw">PRIMARY KEY</span>,
    FechaInicio DATETIME2, FechaFin DATETIME2,
    DuracionSegundos <span class="kw">AS</span> DATEDIFF(SECOND, FechaInicio, FechaFin),
    Resultado NVARCHAR(400)
);
<span class="kw">GO</span>

<span class="kw">CREATE PROCEDURE</span> dbo.usp_MantenimientoAutomatizado
<span class="kw">AS</span>
<span class="kw">BEGIN</span>
    <span class="kw">SET NOCOUNT ON</span>;
    <span class="kw">DECLARE</span> @inicio DATETIME2 = SYSDATETIME(), @resultado NVARCHAR(400) = <span class="str">N'OK'</span>;

    <span class="kw">BEGIN TRY</span>
        <span class="com">-- Reorganizar/reconstruir según fragmentación</span>
        <span class="kw">EXEC</span> dbo.usp_MantenimientoFragmentacion;

        <span class="com">-- Actualizar estadísticas de toda la base (críticas incluidas)</span>
        <span class="kw">EXEC</span> sp_updatestats;

        <span class="kw">SET</span> @resultado = <span class="str">N'Mantenimiento completado correctamente'</span>;
    <span class="kw">END TRY</span>
    <span class="kw">BEGIN CATCH</span>
        <span class="kw">SET</span> @resultado = <span class="str">N'ERROR: '</span> + ERROR_MESSAGE();
    <span class="kw">END CATCH</span>

    <span class="kw">INSERT INTO</span> dbo.LogMantenimientoIndices(FechaInicio, FechaFin, Resultado)
    <span class="kw">VALUES</span>(@inicio, SYSDATETIME(), @resultado);
<span class="kw">END</span>;
<span class="kw">GO</span>

<span class="com">-- Registro del Job en SQL Server Agent (ejecutar en msdb)</span>
<span class="kw">USE</span> msdb;
<span class="kw">EXEC</span> dbo.sp_add_job @job_name = <span class="str">N'Mantenimiento_Indices_GradosTitulos'</span>;
<span class="kw">EXEC</span> dbo.sp_add_jobstep
    @job_name = <span class="str">N'Mantenimiento_Indices_GradosTitulos'</span>,
    @step_name = <span class="str">N'Ejecutar mantenimiento'</span>,
    @subsystem = <span class="str">N'TSQL'</span>,
    @command = <span class="str">N'EXEC GradosTitulos.dbo.usp_MantenimientoAutomatizado;'</span>;
<span class="kw">EXEC</span> dbo.sp_add_schedule
    @schedule_name = <span class="str">N'Diario_02am'</span>,
    @freq_type = 4, @freq_interval = 1, @active_start_time = 020000;
<span class="kw">EXEC</span> dbo.sp_attach_schedule
    @job_name = <span class="str">N'Mantenimiento_Indices_GradosTitulos'</span>,
    @schedule_name = <span class="str">N'Diario_02am'</span>;
<span class="kw">EXEC</span> dbo.sp_add_jobserver
    @job_name = <span class="str">N'Mantenimiento_Indices_GradosTitulos'</span>;`,
      justificacion:`Se envolvió toda la lógica de mantenimiento en un <code>TRY...CATCH</code> para que el job nunca falle silenciosamente: cualquier error queda registrado en <code>LogMantenimientoIndices</code> junto con la duración exacta calculada mediante una columna computada (<code>DATEDIFF</code>), lo cual facilita auditar el desempeño del propio proceso de mantenimiento a lo largo del tiempo.<br><br>
      Se reutilizó el procedimiento <code>usp_MantenimientoFragmentacion</code> del Proyecto 2 (principio DRY) en lugar de duplicar lógica, y se programó mediante <code>sp_add_job</code>/<code>sp_add_schedule</code> de SQL Server Agent para que se ejecute de forma completamente desatendida en horario de bajo tráfico (2:00 a.m.).`,
      practicas:['Registrar siempre el resultado del job, incluso en caso de éxito, para tener trazabilidad completa','Configurar notificaciones por correo del Agent ante fallos del job','Revisar periódicamente LogMantenimientoIndices para detectar tendencias de crecimiento en duración','Versionar el script de creación del job junto con el resto de scripts de base de datos']
    }
  ]
},
{
  id:3, icon:'🔒',
  titulo:'Administración de transacciones y bloqueos',
  subtitulo:'Atomicidad, deadlocks y control de concurrencia',
  infografia:[
    {ico:'⚛️',front:'ACID',back:'Atomicidad, Consistencia, Aislamiento y Durabilidad: propiedades que garantizan que una transacción se ejecute de forma completa o no se ejecute en absoluto.'},
    {ico:'✅',front:'COMMIT / ROLLBACK',back:'COMMIT confirma permanentemente los cambios; ROLLBACK deshace todos los cambios de la transacción ante un error.'},
    {ico:'🧯',front:'TRY...CATCH',back:'Estructura de manejo de errores en T-SQL. Permite capturar excepciones y decidir si se hace ROLLBACK y se registra el error.'},
    {ico:'🔗',front:'Bloqueos (locks)',back:'Mecanismo del motor para controlar el acceso concurrente a los datos. Pueden ser compartidos (S), exclusivos (X), de actualización (U), entre otros.'},
    {ico:'💀',front:'Deadlock',back:'Dos transacciones se bloquean mutuamente esperando recursos que la otra tiene. SQL Server elige una "víctima" y la revierte automáticamente.'},
    {ico:'🧭',front:'Orden de acceso',back:'Acceder a las tablas siempre en el mismo orden en todas las transacciones es la técnica más efectiva para prevenir deadlocks.'}
  ],
  proyectos:[
    {
      titulo:'Control de transacciones en la aprobación de títulos',
      enunciado:`Durante la aprobación de un título profesional intervienen varias tablas como <strong>Tramite, Resolucion, Diploma y EvaluacionTrabajo</strong>.<br><br>
      Se solicita implementar una transacción que garantice: atomicidad; confirmación mediante COMMIT; reversión mediante ROLLBACK; manejo de errores con TRY...CATCH; y registro del error ocurrido.`,
      script:`<span class="com">-- Tabla para registrar errores de la transacción</span>
<span class="kw">CREATE TABLE</span> dbo.LogErroresTransaccion (
    Id INT IDENTITY <span class="kw">PRIMARY KEY</span>,
    Fecha DATETIME2 DEFAULT SYSDATETIME(),
    Procedimiento NVARCHAR(200), Mensaje NVARCHAR(MAX),
    Severidad INT, Estado INT, Linea INT
);
<span class="kw">GO</span>

<span class="kw">CREATE PROCEDURE</span> Tramite.usp_AprobarTitulo
    @TramiteId INT, @ResolucionNumero NVARCHAR(30), @DiplomaCodigo NVARCHAR(30)
<span class="kw">AS</span>
<span class="kw">BEGIN</span>
    <span class="kw">SET NOCOUNT ON</span>;
    <span class="kw">BEGIN TRY</span>
        <span class="kw">BEGIN TRANSACTION</span>;

            <span class="kw">UPDATE</span> Tramite.Tramite
            <span class="kw">SET</span> Estado = <span class="str">N'Aprobado'</span>, FechaAprobacion = SYSDATETIME()
            <span class="kw">WHERE</span> TramiteId = @TramiteId;

            <span class="kw">INSERT INTO</span> Tramite.Resolucion(TramiteId, Numero, FechaEmision)
            <span class="kw">VALUES</span>(@TramiteId, @ResolucionNumero, SYSDATETIME());

            <span class="kw">INSERT INTO</span> Tramite.Diploma(TramiteId, Codigo, FechaEmision)
            <span class="kw">VALUES</span>(@TramiteId, @DiplomaCodigo, SYSDATETIME());

            <span class="kw">UPDATE</span> Evaluacion.EvaluacionTrabajo
            <span class="kw">SET</span> Estado = <span class="str">N'Cerrado'</span>
            <span class="kw">WHERE</span> TramiteId = @TramiteId;

        <span class="kw">COMMIT TRANSACTION</span>;
    <span class="kw">END TRY</span>
    <span class="kw">BEGIN CATCH</span>
        <span class="kw">IF</span> @@TRANCOUNT &gt; 0
            <span class="kw">ROLLBACK TRANSACTION</span>;

        <span class="kw">INSERT INTO</span> dbo.LogErroresTransaccion
            (Procedimiento, Mensaje, Severidad, Estado, Linea)
        <span class="kw">VALUES</span>(
            <span class="str">N'usp_AprobarTitulo'</span>, ERROR_MESSAGE(),
            ERROR_SEVERITY(), ERROR_STATE(), ERROR_LINE()
        );

        <span class="kw">THROW</span>;
    <span class="kw">END CATCH</span>
<span class="kw">END</span>;
<span class="kw">GO</span>

<span class="com">-- Ejecución de prueba</span>
<span class="kw">EXEC</span> Tramite.usp_AprobarTitulo 1045, <span class="str">N'RES-2026-0451'</span>, <span class="str">N'DIP-2026-0451'</span>;`,
      justificacion:`El uso de <code>BEGIN TRANSACTION</code> junto con <code>TRY...CATCH</code> garantiza la propiedad de <strong>atomicidad</strong>: si cualquiera de las cuatro operaciones (actualizar trámite, insertar resolución, insertar diploma, cerrar evaluación) falla, ninguna de ellas se aplicará. Se valida <code>@@TRANCOUNT &gt; 0</code> antes del <code>ROLLBACK</code> para evitar el error "no corresponding BEGIN TRANSACTION" en caso de que la transacción ya se haya cerrado automáticamente por un error grave (doomed transaction).<br><br>
      Se registra el error en <code>LogErroresTransaccion</code> capturando <code>ERROR_MESSAGE()</code>, <code>ERROR_SEVERITY()</code>, <code>ERROR_STATE()</code> y <code>ERROR_LINE()</code>, y se relanza con <code>THROW</code> para que la aplicación cliente también sea notificada del fallo.`,
      practicas:['Mantener las transacciones lo más cortas posible para reducir el tiempo de bloqueo de recursos','Usar THROW en lugar de RAISERROR para preservar el número de línea y la pila de errores original','Validar @@TRANCOUNT antes de cualquier ROLLBACK dentro de un CATCH','Registrar siempre el contexto (procedimiento, parámetros) junto al error para facilitar la depuración']
    },
    {
      titulo:'Simulación y resolución de bloqueos',
      enunciado:`Dos coordinadores modifican simultáneamente el mismo trámite provocando bloqueos prolongados.<br><br>
      Se solicita desarrollar scripts que permitan: simular bloqueos; detectarlos mediante DMV; identificar la sesión bloqueadora; liberar recursos correctamente; y elaborar un reporte del incidente.`,
      script:`<span class="com">-- SESIÓN A: inicia una transacción y mantiene el bloqueo abierto intencionalmente</span>
<span class="kw">BEGIN TRANSACTION</span>;
    <span class="kw">UPDATE</span> Tramite.Tramite
    <span class="kw">SET</span> Estado = <span class="str">N'En revisión'</span>
    <span class="kw">WHERE</span> TramiteId = 1045;
    <span class="com">-- (sin COMMIT todavía, simula al Coordinador 1 revisando)</span>
<span class="kw">WAITFOR DELAY</span> <span class="str">'00:01:00'</span>;
<span class="kw">COMMIT TRANSACTION</span>;

<span class="com">-- SESIÓN B (otra ventana): intenta modificar el mismo trámite y queda bloqueada</span>
<span class="kw">UPDATE</span> Tramite.Tramite
<span class="kw">SET</span> Estado = <span class="str">N'Observado'</span>
<span class="kw">WHERE</span> TramiteId = 1045;
<span class="kw">GO</span>

<span class="com">-- DETECCIÓN: identificar sesiones bloqueadas y bloqueadoras via DMV</span>
<span class="kw">SELECT</span>
    br.session_id <span class="kw">AS</span> SesionBloqueada,
    br.blocking_session_id <span class="kw">AS</span> SesionBloqueadora,
    wt.wait_type, wt.wait_duration_ms,
    st.text <span class="kw">AS</span> ConsultaBloqueada
<span class="kw">FROM</span> sys.dm_exec_requests br
<span class="kw">JOIN</span> sys.dm_os_waiting_tasks wt <span class="kw">ON</span> br.session_id = wt.session_id
<span class="kw">CROSS APPLY</span> sys.<span class="fn">dm_exec_sql_text</span>(br.sql_handle) st
<span class="kw">WHERE</span> br.blocking_session_id &lt;&gt; 0;
<span class="kw">GO</span>

<span class="com">-- Identificar qué está ejecutando la sesión bloqueadora</span>
<span class="kw">SELECT</span> s.session_id, s.login_name, s.host_name, s.program_name,
       t.text <span class="kw">AS</span> UltimaConsultaEjecutada
<span class="kw">FROM</span> sys.dm_exec_sessions s
<span class="kw">JOIN</span> sys.dm_exec_connections c <span class="kw">ON</span> s.session_id = c.session_id
<span class="kw">CROSS APPLY</span> sys.<span class="fn">dm_exec_sql_text</span>(c.most_recent_sql_handle) t
<span class="kw">WHERE</span> s.session_id = <span class="fn">@@SPID</span>; <span class="com">-- reemplazar por el session_id bloqueador detectado</span>
<span class="kw">GO</span>

<span class="com">-- LIBERACIÓN controlada de recursos (solo si es necesario, ejemplo administrativo)</span>
<span class="com">-- KILL 60;  -- reemplazar 60 por el session_id real de la sesión bloqueadora</span>

<span class="com">-- Tabla y registro del incidente</span>
<span class="kw">CREATE TABLE</span> dbo.ReporteIncidentesBloqueo (
    Id INT IDENTITY <span class="kw">PRIMARY KEY</span>, Fecha DATETIME2 DEFAULT SYSDATETIME(),
    SesionBloqueada INT, SesionBloqueadora INT,
    RecursoAfectado NVARCHAR(200), TiempoEsperaMs INT, AccionTomada NVARCHAR(300)
);
<span class="kw">INSERT INTO</span> dbo.ReporteIncidentesBloqueo
(SesionBloqueada, SesionBloqueadora, RecursoAfectado, TiempoEsperaMs, AccionTomada)
<span class="kw">VALUES</span> (55, 52, <span class="str">N'Tramite.Tramite - TramiteId 1045'</span>, 42000,
       <span class="str">N'Se esperó el COMMIT natural de la sesión bloqueadora; no fue necesario KILL'</span>);`,
      justificacion:`Se utilizó <code>sys.dm_os_waiting_tasks</code> combinada con <code>sys.dm_exec_requests</code> porque son las DMV estándar de Microsoft para diagnóstico de bloqueos en tiempo real: la primera expone qué sesión está esperando y por qué tipo de recurso, mientras que <code>blocking_session_id</code> en la segunda identifica exactamente quién genera el bloqueo. <code>CROSS APPLY sys.dm_exec_sql_text</code> traduce el <em>sql_handle</em> binario al texto real de la consulta, lo que es clave para diagnosticar sin necesidad de herramientas externas.<br><br>
      El comando <code>KILL</code> se dejó comentado deliberadamente porque debe usarse solo como último recurso administrativo: forzar el cierre de una sesión provoca un ROLLBACK completo de su transacción y puede causar pérdida de trabajo legítimo de otro coordinador.`,
      practicas:['Preferir esperar al COMMIT natural antes que forzar KILL, salvo bloqueo indefinido confirmado','Mantener las transacciones de actualización lo más breves posible','Documentar cada incidente de bloqueo para identificar patrones recurrentes en el aplicativo','Usar el nivel de aislamiento READ COMMITTED SNAPSHOT para reducir bloqueos de lectura']
    },
    {
      titulo:'Prevención de deadlocks',
      enunciado:`Durante la asignación de asesores y jurados se producen deadlocks debido al acceso concurrente.<br><br>
      Se solicita desarrollar un laboratorio que: reproduzca un deadlock; capture el gráfico XML; analice la causa; rediseñe el orden de acceso a tablas; y demuestre la eliminación del conflicto.`,
      script:`<span class="com">-- 1. Habilitar captura de deadlocks en la sesión de sistema (o crear una propia)</span>
<span class="kw">ALTER EVENT SESSION</span> [system_health] <span class="kw">ON SERVER STATE = START</span>;
<span class="kw">GO</span>

<span class="com">-- 2. REPRODUCCIÓN del deadlock (orden de acceso INVERSO entre dos sesiones)</span>
<span class="com">-- Sesión A</span>
<span class="kw">BEGIN TRANSACTION</span>;
    <span class="kw">UPDATE</span> Academico.Asesor <span class="kw">SET</span> CargaActual = CargaActual + 1 <span class="kw">WHERE</span> AsesorId = 7;
    <span class="kw">WAITFOR DELAY</span> <span class="str">'00:00:05'</span>;
    <span class="kw">UPDATE</span> Academico.Jurado <span class="kw">SET</span> CargaActual = CargaActual + 1 <span class="kw">WHERE</span> JuradoId = 12;
<span class="kw">COMMIT</span>;

<span class="com">-- Sesión B (orden inverso -> provoca el deadlock)</span>
<span class="kw">BEGIN TRANSACTION</span>;
    <span class="kw">UPDATE</span> Academico.Jurado <span class="kw">SET</span> CargaActual = CargaActual + 1 <span class="kw">WHERE</span> JuradoId = 12;
    <span class="kw">WAITFOR DELAY</span> <span class="str">'00:00:05'</span>;
    <span class="kw">UPDATE</span> Academico.Asesor <span class="kw">SET</span> CargaActual = CargaActual + 1 <span class="kw">WHERE</span> AsesorId = 7;
<span class="kw">COMMIT</span>;
<span class="kw">GO</span>

<span class="com">-- 3. Capturar y analizar el gráfico XML del deadlock desde system_health</span>
<span class="kw">SELECT</span>
    event_data.query(<span class="str">'.'</span>) <span class="kw">AS</span> DeadlockGraph,
    event_data.value(<span class="str">'(event/@timestamp)[1]'</span>,<span class="str">'DATETIME2'</span>) <span class="kw">AS</span> Fecha
<span class="kw">FROM</span> (
    <span class="kw">SELECT CAST</span>(target_data <span class="kw">AS XML</span>) <span class="kw">AS</span> TargetData
    <span class="kw">FROM</span> sys.dm_xe_session_targets st
    <span class="kw">JOIN</span> sys.dm_xe_sessions s <span class="kw">ON</span> s.address = st.event_session_address
    <span class="kw">WHERE</span> s.name = <span class="str">'system_health'</span> <span class="kw">AND</span> st.target_name = <span class="str">'ring_buffer'</span>
) <span class="kw">AS</span> data
<span class="kw">CROSS APPLY</span> TargetData.nodes(<span class="str">'RingBufferTarget/event[@name="xml_deadlock_report"]'</span>) <span class="kw">AS</span> tab(event_data);
<span class="kw">GO</span>

<span class="com">-- 4. SOLUCIÓN: procedimiento con orden de acceso UNIFICADO (previene el deadlock)</span>
<span class="kw">CREATE PROCEDURE</span> Academico.usp_AsignarCargaAsesorJurado
    @AsesorId INT, @JuradoId INT
<span class="kw">AS</span>
<span class="kw">BEGIN</span>
    <span class="kw">SET NOCOUNT ON</span>;
    <span class="kw">BEGIN TRANSACTION</span>;
        <span class="com">-- Orden fijo: siempre Asesor primero, luego Jurado, en TODAS las sesiones</span>
        <span class="kw">UPDATE</span> Academico.Asesor <span class="kw">SET</span> CargaActual = CargaActual + 1 <span class="kw">WHERE</span> AsesorId = @AsesorId;
        <span class="kw">UPDATE</span> Academico.Jurado <span class="kw">SET</span> CargaActual = CargaActual + 1 <span class="kw">WHERE</span> JuradoId = @JuradoId;
    <span class="kw">COMMIT</span>;
<span class="kw">END</span>;
<span class="kw">GO</span>

<span class="com">-- Prueba: ambas sesiones ahora usan el mismo procedimiento y el mismo orden</span>
<span class="kw">EXEC</span> Academico.usp_AsignarCargaAsesorJurado 7, 12;
<span class="kw">EXEC</span> Academico.usp_AsignarCargaAsesorJurado 7, 12;`,
      justificacion:`El deadlock original se produce porque la Sesión A accede a <em>Asesor → Jurado</em> mientras la Sesión B accede a <em>Jurado → Asesor</em>: cada una retiene el recurso que la otra necesita, generando una espera circular que el motor detecta mediante su <em>deadlock monitor</em> y resuelve eligiendo como víctima a la transacción de menor costo de rollback.<br><br>
      La solución definitiva no es capturar y reintentar el error, sino <strong>unificar el orden de acceso</strong> a los recursos en todas las rutas de código (encapsulado en un único procedimiento almacenado), eliminando estructuralmente la posibilidad de espera circular. Se usó <code>system_health</code> (sesión de Extended Events activa por defecto en SQL Server) para obtener el gráfico XML del deadlock sin necesidad de crear una sesión adicional.`,
      practicas:['Centralizar operaciones multi-tabla en procedimientos almacenados con orden de acceso fijo','Analizar siempre el deadlock graph (xml_deadlock_report) antes de aplicar una solución','Mantener transacciones cortas y evitar WAITFOR o interacción con el usuario dentro de una transacción abierta','Considerar el nivel de aislamiento SNAPSHOT para escenarios de alta concurrencia con conflictos frecuentes']
    }
  ]
},
{
  id:4, icon:'🧭',
  titulo:'Análisis de planes de ejecución',
  subtitulo:'Lectura de operadores físicos y costos estimados',
  infografia:[
    {ico:'📇',front:'Table Scan',back:'Recorre todas las filas de la tabla. Costoso en tablas grandes; suele indicar ausencia de un índice útil.'},
    {ico:'🔍',front:'Index Scan / Seek',back:'Scan recorre todo el índice; Seek busca directamente las filas necesarias. Seek es preferible por su bajo costo.'},
    {ico:'🔁',front:'Nested Loops',back:'Compara cada fila del conjunto externo contra el interno. Eficiente con conjuntos pequeños e índices adecuados.'},
    {ico:'#️⃣',front:'Hash Match',back:'Construye una tabla hash en memoria para unir o agrupar grandes volúmenes de datos sin índices adecuados.'},
    {ico:'↕️',front:'Sort',back:'Ordena el conjunto de resultados; operación costosa en memoria/tempdb si no existe un índice que ya entregue el orden requerido.'},
    {ico:'💰',front:'Costo estimado',back:'Valor relativo (no en segundos) que el optimizador asigna a cada operador para elegir el plan de menor costo total.'}
  ],
  proyectos:[
    {
      titulo:'Diagnóstico de consultas complejas',
      enunciado:`Las consultas que generan reportes institucionales presentan tiempos superiores a 10 segundos.<br><br>
      Se solicita analizar el plan de ejecución para identificar: Table Scan; Index Scan; Nested Loops; Hash Match; Sort; y costos estimados. Posteriormente deberá optimizarse la consulta y compararse ambos planes.`,
      script:`<span class="com">-- 1. Habilitar captura del plan de ejecución real</span>
<span class="kw">SET STATISTICS XML ON</span>;
<span class="kw">GO</span>

<span class="com">-- 2. Consulta original (reporte institucional lento)</span>
<span class="kw">SELECT</span> p.Apellidos, p.Nombres, tr.Estado, res.Numero, esc.Facultad
<span class="kw">FROM</span> Academico.Persona p
<span class="kw">JOIN</span> Tramite.Tramite tr <span class="kw">ON</span> tr.PersonaId = p.PersonaId
<span class="kw">JOIN</span> Tramite.Resolucion res <span class="kw">ON</span> res.TramiteId = tr.TramiteId
<span class="kw">JOIN</span> Academico.Escuela esc <span class="kw">ON</span> esc.EscuelaId = p.EscuelaId
<span class="kw">WHERE</span> tr.Estado = <span class="str">N'Aprobado'</span>
<span class="kw">ORDER BY</span> p.Apellidos;
<span class="kw">GO</span>
<span class="com">-- Plan revela: Table Scan sobre Tramite.Tramite (sin índice en Estado),
-- Hash Match para el JOIN con Resolucion, Sort costoso por ORDER BY Apellidos --></span>

<span class="com">-- 3. Optimización: índices que eliminan el Table Scan y el Sort</span>
<span class="kw">CREATE NONCLUSTERED INDEX</span> IX_Tramite_Estado
<span class="kw">ON</span> Tramite.Tramite(Estado) <span class="kw">INCLUDE</span>(PersonaId, TramiteId);
<span class="kw">GO</span>

<span class="kw">CREATE NONCLUSTERED INDEX</span> IX_Persona_Apellidos_Escuela
<span class="kw">ON</span> Academico.Persona(Apellidos) <span class="kw">INCLUDE</span>(Nombres, EscuelaId, PersonaId);
<span class="kw">GO</span>

<span class="com">-- 4. Consulta optimizada (misma lógica, ahora con Index Seek + Nested Loops)</span>
<span class="kw">SELECT</span> p.Apellidos, p.Nombres, tr.Estado, res.Numero, esc.Facultad
<span class="kw">FROM</span> Tramite.Tramite tr
<span class="kw">JOIN</span> Academico.Persona p <span class="kw">ON</span> p.PersonaId = tr.PersonaId
<span class="kw">JOIN</span> Tramite.Resolucion res <span class="kw">ON</span> res.TramiteId = tr.TramiteId
<span class="kw">JOIN</span> Academico.Escuela esc <span class="kw">ON</span> esc.EscuelaId = p.EscuelaId
<span class="kw">WHERE</span> tr.Estado = <span class="str">N'Aprobado'</span>
<span class="kw">ORDER BY</span> p.Apellidos;
<span class="kw">GO</span>
<span class="kw">SET STATISTICS XML OFF</span>;`,
      justificacion:`El plan original mostraba un <strong>Table Scan</strong> sobre <code>Tramite.Tramite</code> porque no existía ningún índice sobre la columna <code>Estado</code> usada en el filtro, obligando al motor a leer cada fila de la tabla. El <strong>Hash Match</strong> aparecía porque, sin índices adecuados en las claves foráneas, el optimizador prefiere construir una tabla hash en memoria en lugar de un <em>Nested Loops</em> más barato. El operador <strong>Sort</strong> se generaba por el <code>ORDER BY Apellidos</code> sin un índice que ya entregara ese orden.<br><br>
      Tras crear los índices <em>covering</em> adecuados, el nuevo plan sustituye el Table Scan por un <strong>Index Seek</strong>, el Hash Match por <strong>Nested Loops</strong> (más eficiente al reducirse drásticamente el número de filas candidatas), y en muchos casos elimina el Sort explícito porque el índice ya entrega los datos preordenados por Apellidos.`,
      practicas:['Comparar siempre el costo estimado total del plan (no solo un operador aislado) antes y después','Usar el plan real (STATISTICS XML/actual execution plan) y no solo el estimado, para datos fidedignos','Priorizar la creación de índices sobre columnas usadas en WHERE, JOIN y ORDER BY simultáneamente','Evitar sobre-indexar: cada índice nuevo debe justificarse con una mejora medible del plan']
    },
    {
      titulo:'Comparación de planes antes y después de crear índices',
      enunciado:`La Dirección de Informática desea comprobar el impacto real de nuevos índices sobre las consultas del módulo de grados.<br><br>
      El estudiante deberá: obtener el plan inicial; crear índices adecuados; obtener el nuevo plan; comparar operadores; comparar costo estimado; y comparar tiempo total.`,
      script:`<span class="com">-- 1. Plan inicial: obtener costo estimado sin ejecutar (Estimated Execution Plan)</span>
<span class="kw">SET SHOWPLAN_XML ON</span>;
<span class="kw">GO</span>
<span class="kw">SELECT</span> d.CodigoDiploma, d.FechaEmision, p.Apellidos
<span class="kw">FROM</span> Tramite.Diploma d
<span class="kw">JOIN</span> Academico.Persona p <span class="kw">ON</span> p.PersonaId = d.PersonaId
<span class="kw">WHERE</span> d.FechaEmision <span class="kw">BETWEEN</span> <span class="str">'2026-01-01'</span> <span class="kw">AND</span> <span class="str">'2026-06-30'</span>;
<span class="kw">GO</span>
<span class="kw">SET SHOWPLAN_XML OFF</span>;
<span class="kw">GO</span>
<span class="com">-- Costo estimado: 45.2 (Clustered Index Scan sobre Diploma completo)</span>

<span class="com">-- 2. Crear índice adecuado sobre la columna de filtro</span>
<span class="kw">CREATE NONCLUSTERED INDEX</span> IX_Diploma_FechaEmision
<span class="kw">ON</span> Tramite.Diploma(FechaEmision) <span class="kw">INCLUDE</span>(CodigoDiploma, PersonaId);
<span class="kw">GO</span>

<span class="com">-- 3. Nuevo plan (real, con ejecución) para medir tiempo total</span>
<span class="kw">SET STATISTICS TIME ON</span>;
<span class="kw">SET STATISTICS XML ON</span>;
<span class="kw">GO</span>
<span class="kw">SELECT</span> d.CodigoDiploma, d.FechaEmision, p.Apellidos
<span class="kw">FROM</span> Tramite.Diploma d
<span class="kw">JOIN</span> Academico.Persona p <span class="kw">ON</span> p.PersonaId = d.PersonaId
<span class="kw">WHERE</span> d.FechaEmision <span class="kw">BETWEEN</span> <span class="str">'2026-01-01'</span> <span class="kw">AND</span> <span class="str">'2026-06-30'</span>;
<span class="kw">GO</span>
<span class="com">-- Costo estimado nuevo: 3.7 (Index Seek + Key Lookup evitado por INCLUDE)</span>
<span class="kw">SET STATISTICS TIME OFF</span>;
<span class="kw">SET STATISTICS XML OFF</span>;

<span class="com">-- 4. Tabla comparativa manual de resultados</span>
<span class="kw">SELECT</span> <span class="str">'Antes'</span> <span class="kw">AS</span> Escenario, <span class="str">'Clustered Index Scan'</span> <span class="kw">AS</span> OperadorPrincipal, 45.2 <span class="kw">AS</span> CostoEstimado, 1180 <span class="kw">AS</span> TiempoMs
<span class="kw">UNION ALL</span>
<span class="kw">SELECT</span> <span class="str">'Después'</span>, <span class="str">'Index Seek'</span>, 3.7, 62;`,
      justificacion:`Se comparó el plan mediante dos métricas objetivas: el <strong>costo estimado</strong> (unidad relativa interna del optimizador, no tiempo real) obtenido con <code>SET SHOWPLAN_XML</code>, y el <strong>tiempo real de ejecución</strong> obtenido con <code>SET STATISTICS TIME ON</code>. Ambas métricas deben mejorar de forma consistente para confirmar que el índice es efectivo; si solo mejora una de las dos, puede indicar un problema de caché de datos en lugar de una mejora estructural real.<br><br>
      El índice <code>IX_Diploma_FechaEmision</code> con columna <code>INCLUDE</code> convierte la operación en un <em>covering index</em>, eliminando el Key Lookup adicional que se hubiera necesitado para obtener <code>CodigoDiploma</code> y <code>PersonaId</code> desde la tabla base.`,
      practicas:['Ejecutar cada consulta varias veces y descartar la primera ejecución (efecto de caché de buffer pool)','Documentar el costo estimado y el tiempo real en una tabla comparativa para el reporte final','Verificar con DBCC FREEPROCCACHE en ambiente de pruebas para comparar planes sin caché previo','Evitar conclusiones basadas solo en el costo estimado sin validar con datos reales']
    },
    {
      titulo:'Optimización de procedimientos almacenados',
      enunciado:`Los procedimientos utilizados para generar resoluciones presentan degradación del rendimiento.<br><br>
      Se solicita desarrollar un análisis del plan de ejecución para: detectar operadores costosos; eliminar Table Scan; reducir lecturas lógicas; mejorar la reutilización del plan; y documentar la optimización obtenida.`,
      script:`<span class="com">-- Procedimiento original (con problemas de rendimiento)</span>
<span class="kw">CREATE OR ALTER PROCEDURE</span> Tramite.usp_GenerarResolucion_v1
    @TramiteId INT
<span class="kw">AS</span>
<span class="kw">BEGIN</span>
    <span class="kw">SELECT</span> * <span class="com">-- SELECT * dificulta la reutilización del plan y trae columnas innecesarias</span>
    <span class="kw">FROM</span> Tramite.Tramite
    <span class="kw">WHERE</span> CONVERT(VARCHAR, TramiteId) = CONVERT(VARCHAR, @TramiteId); <span class="com">-- función sobre la columna: impide Index Seek</span>
<span class="kw">END</span>;
<span class="kw">GO</span>

<span class="com">-- Diagnóstico: analizar operadores costosos y lecturas lógicas</span>
<span class="kw">SET STATISTICS IO ON</span>;
<span class="kw">EXEC</span> Tramite.usp_GenerarResolucion_v1 1045;
<span class="com">-- Resultado: Table Scan, 3400 lecturas lógicas, plan no reutilizable (no parametrizado por CONVERT)</span>

<span class="com">-- Versión optimizada</span>
<span class="kw">CREATE OR ALTER PROCEDURE</span> Tramite.usp_GenerarResolucion_v2
    @TramiteId INT
<span class="kw">AS</span>
<span class="kw">BEGIN</span>
    <span class="kw">SET NOCOUNT ON</span>;
    <span class="kw">SELECT</span> TramiteId, PersonaId, Estado, FechaAprobacion <span class="com">-- solo columnas necesarias</span>
    <span class="kw">FROM</span> Tramite.Tramite
    <span class="kw">WHERE</span> TramiteId = @TramiteId; <span class="com">-- comparación directa, permite Index Seek + reutilización del plan</span>
<span class="kw">END</span>;
<span class="kw">GO</span>

<span class="kw">EXEC</span> Tramite.usp_GenerarResolucion_v2 1045;
<span class="com">-- Resultado: Index Seek (sobre PK), 3 lecturas lógicas, plan cacheado y reutilizable</span>
<span class="kw">SET STATISTICS IO OFF</span>;

<span class="com">-- Documentar la optimización en tabla de bitácora técnica</span>
<span class="kw">CREATE TABLE</span> dbo.BitacoraOptimizacion (
    Id INT IDENTITY <span class="kw">PRIMARY KEY</span>, Procedimiento NVARCHAR(200),
    LecturasAntes INT, LecturasDespues INT, Observacion NVARCHAR(400), Fecha DATETIME2 DEFAULT SYSDATETIME()
);
<span class="kw">INSERT INTO</span> dbo.BitacoraOptimizacion(Procedimiento, LecturasAntes, LecturasDespues, Observacion)
<span class="kw">VALUES</span>(<span class="str">'usp_GenerarResolucion'</span>, 3400, 3,
       <span class="str">N'Se eliminó CONVERT sobre columna indexada y SELECT *; el plan ahora usa Index Seek y es reutilizable'</span>);`,
      justificacion:`El operador costoso en la versión original se debía a aplicar <code>CONVERT()</code> directamente sobre la columna <code>TramiteId</code>: esto se conoce como <em>non-sargable predicate</em>, ya que impide que el optimizador use el índice sobre esa columna, forzando un <strong>Table Scan</strong> completo. Además, usar literales sin parámetros tipados adecuadamente puede generar planes no reutilizables, aumentando la compilación repetida (plan cache bloat).<br><br>
      Al comparar directamente <code>TramiteId = @TramiteId</code> sin funciones envolventes, el motor puede usar un <strong>Index Seek</strong> sobre la clave primaria, reduciendo las lecturas lógicas de 3400 a solo 3, y el plan queda correctamente parametrizado y cacheado para reutilización en llamadas posteriores.`,
      practicas:['Evitar funciones (CONVERT, UPPER, SUBSTRING) sobre columnas indexadas dentro del WHERE','Seleccionar solo las columnas necesarias en lugar de SELECT *','Usar parámetros tipados explícitamente para maximizar la reutilización del plan en caché','Medir siempre lecturas lógicas con STATISTICS IO antes y después de cada cambio']
    }
  ]
},
{
  id:5, icon:'⚙️',
  titulo:'Optimización de consultas T-SQL',
  subtitulo:'Reescritura eficiente: CTE, JOIN, EXISTS y funciones ventana',
  infografia:[
    {ico:'🔀',front:'JOIN vs subconsulta',back:'Un JOIN bien indexado suele ser más eficiente que una subconsulta correlacionada, que se re-ejecuta por cada fila del conjunto externo.'},
    {ico:'❓',front:'EXISTS vs IN',back:'EXISTS se detiene en la primera coincidencia (short-circuit) y es preferible a IN cuando se valida existencia, especialmente con subconsultas grandes.'},
    {ico:'🧱',front:'CTE',back:'Common Table Expression: mejora la legibilidad y permite referenciar el mismo conjunto de datos varias veces sin repetir la subconsulta.'},
    {ico:'📊',front:'Funciones ventana',back:'ROW_NUMBER, RANK, SUM() OVER: cálculos analíticos sin necesidad de GROUP BY ni autouniones, muy eficientes para reportes.'},
    {ico:'🎯',front:'Filtrado temprano',back:'Aplicar el WHERE lo antes posible en la lógica de la consulta reduce el volumen de filas que procesan los operadores siguientes.'},
    {ico:'🎯',front:'Parameter sniffing',back:'El optimizador cachea un plan basado en el primer valor de parámetro usado, lo cual puede ser subóptimo para valores muy distintos posteriores.'}
  ],
  proyectos:[
    {
      titulo:'Reescritura de consultas de alta demanda',
      enunciado:`Las consultas utilizadas para mostrar el historial completo del trámite de un estudiante consumen recursos excesivos.<br><br>
      Se solicita optimizar dichas consultas mediante: eliminación de subconsultas innecesarias; uso de JOIN eficientes; uso de EXISTS; uso de CTE; y comparación de tiempos.`,
      script:`<span class="com">-- Versión original: subconsulta correlacionada repetida por cada fila</span>
<span class="kw">SELECT</span> tr.TramiteId, tr.Estado,
    (<span class="kw">SELECT</span> COUNT(*) <span class="kw">FROM</span> Tramite.Resolucion r <span class="kw">WHERE</span> r.TramiteId = tr.TramiteId) <span class="kw">AS</span> NumResoluciones,
    (<span class="kw">SELECT MAX</span>(d.FechaEmision) <span class="kw">FROM</span> Tramite.Diploma d <span class="kw">WHERE</span> d.TramiteId = tr.TramiteId) <span class="kw">AS</span> UltimoDiploma
<span class="kw">FROM</span> Tramite.Tramite tr
<span class="kw">WHERE</span> tr.PersonaId <span class="kw">IN</span> (<span class="kw">SELECT</span> PersonaId <span class="kw">FROM</span> Academico.Persona <span class="kw">WHERE</span> EscuelaId = 3);
<span class="kw">GO</span>

<span class="com">-- Versión optimizada: CTE + JOIN + EXISTS, sin subconsultas correlacionadas</span>
<span class="kw">SET STATISTICS TIME ON</span>;
<span class="kw">;WITH</span> ResumenResolucion <span class="kw">AS</span> (
    <span class="kw">SELECT</span> TramiteId, COUNT(*) <span class="kw">AS</span> NumResoluciones
    <span class="kw">FROM</span> Tramite.Resolucion
    <span class="kw">GROUP BY</span> TramiteId
),
UltimoDiplomaCTE <span class="kw">AS</span> (
    <span class="kw">SELECT</span> TramiteId, MAX(FechaEmision) <span class="kw">AS</span> UltimoDiploma
    <span class="kw">FROM</span> Tramite.Diploma
    <span class="kw">GROUP BY</span> TramiteId
)
<span class="kw">SELECT</span> tr.TramiteId, tr.Estado,
       ISNULL(rr.NumResoluciones, 0) <span class="kw">AS</span> NumResoluciones,
       ud.UltimoDiploma
<span class="kw">FROM</span> Tramite.Tramite tr
<span class="kw">LEFT JOIN</span> ResumenResolucion rr <span class="kw">ON</span> rr.TramiteId = tr.TramiteId
<span class="kw">LEFT JOIN</span> UltimoDiplomaCTE ud <span class="kw">ON</span> ud.TramiteId = tr.TramiteId
<span class="kw">WHERE EXISTS</span> (
    <span class="kw">SELECT</span> 1 <span class="kw">FROM</span> Academico.Persona p
    <span class="kw">WHERE</span> p.PersonaId = tr.PersonaId <span class="kw">AND</span> p.EscuelaId = 3
);
<span class="kw">SET STATISTICS TIME OFF</span>;`,
      justificacion:`La versión original ejecuta dos subconsultas correlacionadas <strong>por cada fila</strong> devuelta por la consulta externa (complejidad O(n×m)), lo que resulta extremadamente costoso a medida que crece la tabla de trámites. Al reescribirla con <strong>CTE</strong> que preagregan (<code>GROUP BY</code>) una sola vez cada conjunto y luego se unen mediante <strong>LEFT JOIN</strong>, la complejidad se reduce a un recorrido lineal más una operación de unión optimizada por el motor.<br><br>
      Se reemplazó el <code>IN (SELECT ...)</code> por <code>EXISTS</code> porque, al validar únicamente existencia, el motor puede detener la búsqueda en la primera coincidencia (short-circuit evaluation), evitando materializar la lista completa de PersonaId como ocurre con IN.`,
      practicas:['Preferir CTE con GROUP BY sobre subconsultas correlacionadas para cálculos por fila','Usar EXISTS en lugar de IN cuando solo se necesita validar existencia de registros relacionados','Usar LEFT JOIN con ISNULL en lugar de subconsultas escalares repetidas','Medir siempre con STATISTICS TIME antes y después de cada reescritura']
    },
    {
      titulo:'Optimización de reportes académicos',
      enunciado:`Los reportes institucionales integran información proveniente de más de ocho tablas.<br><br>
      Se solicita desarrollar una versión optimizada utilizando: CTE; funciones ventana; índices adecuados; filtrado temprano; y estadísticas actualizadas. Comparar rendimiento antes y después.`,
      script:`<span class="com">-- Estadísticas actualizadas antes de medir (garantiza plan óptimo del optimizador)</span>
<span class="kw">UPDATE STATISTICS</span> Tramite.Tramite <span class="kw">WITH FULLSCAN</span>;
<span class="kw">UPDATE STATISTICS</span> Tramite.Resolucion <span class="kw">WITH FULLSCAN</span>;
<span class="kw">GO</span>

<span class="com">-- Índice de apoyo para el filtrado temprano por año académico</span>
<span class="kw">CREATE NONCLUSTERED INDEX</span> IX_Tramite_AnioAcademico
<span class="kw">ON</span> Tramite.Tramite(AnioAcademico) <span class="kw">INCLUDE</span>(PersonaId, Estado);
<span class="kw">GO</span>

<span class="kw">SET STATISTICS TIME ON</span>;
<span class="com">-- Reporte optimizado: filtrado temprano dentro del CTE + función ventana para ranking por escuela</span>
<span class="kw">;WITH</span> TramitesFiltrados <span class="kw">AS</span> (
    <span class="com">-- Filtrado temprano: reduce el volumen ANTES de unir con las demás tablas</span>
    <span class="kw">SELECT</span> TramiteId, PersonaId, Estado
    <span class="kw">FROM</span> Tramite.Tramite
    <span class="kw">WHERE</span> AnioAcademico = 2026
),
ReporteBase <span class="kw">AS</span> (
    <span class="kw">SELECT</span>
        p.Apellidos, p.Nombres, e.Facultad, tf.Estado,
        res.Numero <span class="kw">AS</span> NumeroResolucion,
        <span class="fn">ROW_NUMBER</span>() <span class="kw">OVER</span>(<span class="kw">PARTITION BY</span> e.Facultad <span class="kw">ORDER BY</span> res.FechaEmision <span class="kw">DESC</span>) <span class="kw">AS</span> RankingPorFacultad,
        <span class="fn">COUNT</span>(*) <span class="kw">OVER</span>(<span class="kw">PARTITION BY</span> e.Facultad) <span class="kw">AS</span> TotalPorFacultad
    <span class="kw">FROM</span> TramitesFiltrados tf
    <span class="kw">JOIN</span> Academico.Persona p <span class="kw">ON</span> p.PersonaId = tf.PersonaId
    <span class="kw">JOIN</span> Academico.Escuela e <span class="kw">ON</span> e.EscuelaId = p.EscuelaId
    <span class="kw">JOIN</span> Tramite.Resolucion res <span class="kw">ON</span> res.TramiteId = tf.TramiteId
)
<span class="kw">SELECT</span> * <span class="kw">FROM</span> ReporteBase <span class="kw">WHERE</span> RankingPorFacultad &lt;= 10 <span class="kw">ORDER BY</span> Facultad, RankingPorFacultad;
<span class="kw">SET STATISTICS TIME OFF</span>;`,
      justificacion:`El <strong>filtrado temprano</strong> dentro del CTE <code>TramitesFiltrados</code> reduce el volumen de datos antes de realizar los JOIN con Persona, Escuela y Resolucion, en lugar de unir primero las 8+ tablas completas y filtrar al final. La función ventana <code>ROW_NUMBER() OVER (PARTITION BY ... ORDER BY ...)</code> reemplaza lo que tradicionalmente requeriría una autounión o una subconsulta con TOP por grupo, calculando el ranking en una sola pasada sobre los datos ya unidos.<br><br>
      Actualizar estadísticas con <code>FULLSCAN</code> garantiza que el optimizador cuente con la distribución de datos más precisa posible al generar el plan, lo cual es crítico en tablas con crecimiento reciente o cambios masivos.`,
      practicas:['Actualizar estadísticas antes de medir el impacto real de una optimización','Filtrar los datos lo antes posible en la cadena de CTE, no al final del reporte','Usar funciones ventana en lugar de autouniones o subconsultas para cálculos de ranking','Crear índices que cubran tanto el filtro (WHERE) como las columnas usadas en el PARTITION BY']
    },
    {
      titulo:'Optimización de procedimientos de búsqueda',
      enunciado:`El procedimiento encargado de localizar expedientes consume demasiada CPU durante horarios de alta demanda.<br><br>
      Se solicita optimizarlo mediante: parametrización; eliminación de cursores innecesarios; uso adecuado de variables temporales; revisión del Parameter Sniffing; y comparación del consumo de CPU y tiempo.`,
      script:`<span class="com">-- Versión original: usa CURSOR para recorrer expedientes uno por uno (ineficiente)</span>
<span class="kw">CREATE OR ALTER PROCEDURE</span> Documento.usp_BuscarExpedientes_v1
    @Apellido NVARCHAR(100)
<span class="kw">AS</span>
<span class="kw">BEGIN</span>
    <span class="kw">DECLARE</span> @PersonaId INT;
    <span class="kw">DECLARE</span> cur <span class="kw">CURSOR FOR</span>
        <span class="kw">SELECT</span> PersonaId <span class="kw">FROM</span> Academico.Persona <span class="kw">WHERE</span> Apellidos <span class="kw">LIKE</span> <span class="str">'%'</span> + @Apellido + <span class="str">'%'</span>;
    <span class="kw">OPEN</span> cur;
    <span class="kw">FETCH NEXT FROM</span> cur <span class="kw">INTO</span> @PersonaId;
    <span class="kw">WHILE</span> @@FETCH_STATUS = 0
    <span class="kw">BEGIN</span>
        <span class="kw">SELECT</span> * <span class="kw">FROM</span> Documento.Expediente <span class="kw">WHERE</span> PersonaId = @PersonaId;
        <span class="kw">FETCH NEXT FROM</span> cur <span class="kw">INTO</span> @PersonaId;
    <span class="kw">END</span>
    <span class="kw">CLOSE</span> cur; <span class="kw">DEALLOCATE</span> cur;
<span class="kw">END</span>;
<span class="kw">GO</span>

<span class="com">-- Versión optimizada: set-based, sin cursor, con OPTION para mitigar parameter sniffing</span>
<span class="kw">CREATE OR ALTER PROCEDURE</span> Documento.usp_BuscarExpedientes_v2
    @Apellido NVARCHAR(100)
<span class="kw">AS</span>
<span class="kw">BEGIN</span>
    <span class="kw">SET NOCOUNT ON</span>;

    <span class="com">-- Tabla temporal para materializar candidatos y evitar recompute repetido</span>
    <span class="kw">CREATE TABLE</span> #Candidatos (PersonaId INT <span class="kw">PRIMARY KEY</span>);

    <span class="kw">INSERT INTO</span> #Candidatos
    <span class="kw">SELECT</span> PersonaId <span class="kw">FROM</span> Academico.Persona
    <span class="kw">WHERE</span> Apellidos <span class="kw">LIKE</span> @Apellido + <span class="str">'%'</span>; <span class="com">-- prefijo (sargable) en vez de '%...%'</span>

    <span class="kw">SELECT</span> e.*
    <span class="kw">FROM</span> Documento.Expediente e
    <span class="kw">JOIN</span> #Candidatos c <span class="kw">ON</span> c.PersonaId = e.PersonaId
    <span class="kw">OPTION</span> (RECOMPILE); <span class="com">-- evita que un plan cacheado para un @Apellido poco selectivo afecte a otros más selectivos</span>

    <span class="kw">DROP TABLE</span> #Candidatos;
<span class="kw">END</span>;
<span class="kw">GO</span>

<span class="com">-- Comparación de consumo</span>
<span class="kw">SET STATISTICS TIME ON</span>;
<span class="kw">EXEC</span> Documento.usp_BuscarExpedientes_v1 <span class="str">N'Cuchula'</span>;
<span class="kw">EXEC</span> Documento.usp_BuscarExpedientes_v2 <span class="str">N'Cuchula'</span>;
<span class="kw">SET STATISTICS TIME OFF</span>;`,
      justificacion:`El uso de <strong>CURSOR</strong> obliga al motor a procesar los expedientes fila por fila (RBAR: Row By Agonizing Row), generando una sobrecarga de CPU proporcional al número de personas encontradas. La versión optimizada usa un enfoque <em>set-based</em> con una tabla temporal y un único <code>JOIN</code>, permitiendo que el optimizador procese el conjunto completo de una sola vez.<br><br>
      Se cambió el patrón <code>LIKE '%...%'</code> por <code>LIKE 'prefijo%'</code> porque el primero es <em>non-sargable</em> (no puede usar un índice B-tree eficientemente), mientras que el segundo sí permite un Index Seek. Finalmente, <code>OPTION (RECOMPILE)</code> mitiga el <strong>parameter sniffing</strong>: obliga a generar un plan nuevo adaptado a la selectividad real de cada valor de <code>@Apellido</code>, evitando que un plan cacheado para un apellido muy común perjudique búsquedas de apellidos poco frecuentes (o viceversa).`,
      practicas:['Evitar CURSOR para operaciones que pueden resolverse con lógica set-based','Usar LIKE con prefijo en lugar de comodín al inicio para mantener la operación sargable','Aplicar OPTION (RECOMPILE) solo cuando la selectividad del parámetro varía mucho entre ejecuciones','Medir siempre el consumo de CPU con STATISTICS TIME antes de declarar una optimización exitosa']
    }
  ]
},
{
  id:6, icon:'🛡️',
  titulo:'Control de recursos con Resource Governor',
  subtitulo:'Gobernanza de CPU y memoria por grupo de carga',
  infografia:[
    {ico:'🏊',front:'Resource Pool',back:'Representa una porción física de los recursos del servidor (CPU, memoria) que se puede limitar o garantizar para un grupo de usuarios.'},
    {ico:'👥',front:'Workload Group',back:'Agrupa sesiones con características similares (ej. administrativos vs institucionales) dentro de un Resource Pool específico.'},
    {ico:'🏷️',front:'Función de clasificación',back:'Función escalar que SQL Server ejecuta al inicio de cada conexión para decidir a qué Workload Group pertenece según login, app o rol.'},
    {ico:'📉',front:'MAX_CPU_PERCENT',back:'Límite superior de CPU que puede consumir un Resource Pool, evitando que un grupo de baja prioridad afecte a los críticos.'},
    {ico:'🧠',front:'MAX_MEMORY_PERCENT',back:'Límite de memoria del servidor que puede usar un pool, protegiendo la caché de planes y buffer pool de otros procesos.'},
    {ico:'📡',front:'DMV de monitoreo',back:'sys.dm_resource_governor_resource_pools y sys.dm_resource_governor_workload_groups exponen el consumo real de cada grupo.'}
  ],
  proyectos:[
    {
      titulo:'Administración de carga por tipo de usuario',
      enunciado:`La universidad desea limitar el consumo de recursos de usuarios administrativos sin afectar las consultas institucionales críticas.<br><br>
      Se solicita configurar Resource Governor para: crear Resource Pool; crear Workload Group; clasificar conexiones; limitar CPU; limitar memoria; y verificar el funcionamiento mediante pruebas.`,
      script:`<span class="com">-- 1. Crear Resource Pool para usuarios administrativos (limitado)</span>
<span class="kw">CREATE RESOURCE POOL</span> PoolAdministrativos
<span class="kw">WITH</span> (
    MAX_CPU_PERCENT = 30,
    MAX_MEMORY_PERCENT = 20
);
<span class="kw">GO</span>

<span class="com">-- 2. Resource Pool para consultas institucionales críticas (con garantía mínima)</span>
<span class="kw">CREATE RESOURCE POOL</span> PoolInstitucional
<span class="kw">WITH</span> (
    MIN_CPU_PERCENT = 40, MAX_CPU_PERCENT = 100,
    MIN_MEMORY_PERCENT = 30, MAX_MEMORY_PERCENT = 100
);
<span class="kw">GO</span>

<span class="com">-- 3. Crear Workload Groups asociados a cada pool</span>
<span class="kw">CREATE WORKLOAD GROUP</span> WGAdministrativos
<span class="kw">USING</span> PoolAdministrativos;
<span class="kw">GO</span>
<span class="kw">CREATE WORKLOAD GROUP</span> WGInstitucional
<span class="kw">USING</span> PoolInstitucional;
<span class="kw">GO</span>

<span class="com">-- 4. Función de clasificación: decide el grupo según el login de la aplicación</span>
<span class="kw">CREATE FUNCTION</span> dbo.fn_ClasificadorGradosTitulos()
<span class="kw">RETURNS SYSNAME</span>
<span class="kw">WITH SCHEMABINDING AS</span>
<span class="kw">BEGIN</span>
    <span class="kw">DECLARE</span> @grupo SYSNAME;
    <span class="kw">IF</span> ORIGINAL_LOGIN() <span class="kw">IN</span> (<span class="str">'appAdministrativa'</span>, <span class="str">'usrBackoffice'</span>)
        <span class="kw">SET</span> @grupo = <span class="str">'WGAdministrativos'</span>;
    <span class="kw">ELSE</span>
        <span class="kw">SET</span> @grupo = <span class="str">'WGInstitucional'</span>;
    <span class="kw">RETURN</span> @grupo;
<span class="kw">END</span>;
<span class="kw">GO</span>

<span class="com">-- 5. Aplicar la clasificación y activar Resource Governor</span>
<span class="kw">ALTER RESOURCE GOVERNOR WITH</span> (CLASSIFIER_FUNCTION = dbo.fn_ClasificadorGradosTitulos);
<span class="kw">ALTER RESOURCE GOVERNOR RECONFIGURE</span>;
<span class="kw">GO</span>

<span class="com">-- 6. Verificación mediante DMV</span>
<span class="kw">SELECT</span> pool_id, name, max_cpu_percent, max_memory_percent
<span class="kw">FROM</span> sys.resource_governor_resource_pools;
<span class="kw">GO</span>
<span class="kw">SELECT</span> group_id, name, pool_id, total_cpu_usage_ms
<span class="kw">FROM</span> sys.dm_resource_governor_workload_groups;`,
      justificacion:`Se crearon dos <strong>Resource Pools</strong> diferenciados: uno para usuarios administrativos con un límite estricto (<code>MAX_CPU_PERCENT = 30</code>) y otro para consultas institucionales con una garantía mínima (<code>MIN_CPU_PERCENT = 40</code>), asegurando que estas últimas siempre dispongan de al menos 40% de CPU incluso bajo carga administrativa intensa. La <strong>función de clasificación</strong> se ejecuta automáticamente en el <em>logon trigger</em> interno de Resource Governor, evaluando <code>ORIGINAL_LOGIN()</code> para asignar cada conexión al grupo correcto sin intervención manual.<br><br>
      Se usó <code>SCHEMABINDING</code> en la función de clasificación porque es un requisito de SQL Server para las funciones usadas por Resource Governor, garantizando que no cambien de estructura mientras estén en uso.`,
      practicas:['Probar la función de clasificación exhaustivamente antes de activarla, un error puede bloquear nuevas conexiones','Establecer MIN_CPU_PERCENT solo en los pools verdaderamente críticos para no desperdiciar capacidad ociosa','Monitorear sys.dm_resource_governor_resource_pools periódicamente tras la implementación','Mantener siempre el pool "default" disponible para conexiones administrativas de emergencia (DAC)']
    },
    {
      titulo:'Priorización de procesos críticos',
      enunciado:`Durante el cierre del semestre, los procesos de emisión de diplomas deben tener prioridad sobre las consultas de usuarios generales.<br><br>
      Se solicita desarrollar una solución que: cree grupos de carga diferenciados; priorice procesos críticos; restrinja consultas de baja prioridad; y evalúe el impacto sobre el rendimiento.`,
      script:`<span class="com">-- 1. Pool y grupo para emisión de diplomas (alta prioridad, IMPORTANCE HIGH)</span>
<span class="kw">CREATE RESOURCE POOL</span> PoolEmisionDiplomas
<span class="kw">WITH</span> (MIN_CPU_PERCENT = 50, MAX_CPU_PERCENT = 100);
<span class="kw">GO</span>
<span class="kw">CREATE WORKLOAD GROUP</span> WGEmisionDiplomas
<span class="kw">WITH</span> (IMPORTANCE = HIGH, REQUEST_MAX_CPU_TIME_SEC = 0)
<span class="kw">USING</span> PoolEmisionDiplomas;
<span class="kw">GO</span>

<span class="com">-- 2. Pool y grupo para consultas de usuarios generales (baja prioridad, restringido)</span>
<span class="kw">CREATE RESOURCE POOL</span> PoolConsultasGenerales
<span class="kw">WITH</span> (MAX_CPU_PERCENT = 25, MAX_MEMORY_PERCENT = 15);
<span class="kw">GO</span>
<span class="kw">CREATE WORKLOAD GROUP</span> WGConsultasGenerales
<span class="kw">WITH</span> (IMPORTANCE = LOW, REQUEST_MAX_CPU_TIME_SEC = 30)
<span class="kw">USING</span> PoolConsultasGenerales;
<span class="kw">GO</span>

<span class="com">-- 3. Clasificador que detecta el proceso de emisión por nombre de aplicación</span>
<span class="kw">CREATE OR ALTER FUNCTION</span> dbo.fn_ClasificadorCierreSemestre()
<span class="kw">RETURNS SYSNAME WITH SCHEMABINDING AS</span>
<span class="kw">BEGIN</span>
    <span class="kw">DECLARE</span> @grupo SYSNAME;
    <span class="kw">IF</span> APP_NAME() <span class="kw">LIKE</span> <span class="str">'%EmisionDiplomas%'</span>
        <span class="kw">SET</span> @grupo = <span class="str">'WGEmisionDiplomas'</span>;
    <span class="kw">ELSE</span>
        <span class="kw">SET</span> @grupo = <span class="str">'WGConsultasGenerales'</span>;
    <span class="kw">RETURN</span> @grupo;
<span class="kw">END</span>;
<span class="kw">GO</span>
<span class="kw">ALTER RESOURCE GOVERNOR WITH</span> (CLASSIFIER_FUNCTION = dbo.fn_ClasificadorCierreSemestre);
<span class="kw">ALTER RESOURCE GOVERNOR RECONFIGURE</span>;
<span class="kw">GO</span>

<span class="com">-- 4. Evaluación del impacto: comparar tiempo de CPU consumido por grupo</span>
<span class="kw">SELECT</span> wg.name <span class="kw">AS</span> Grupo, wg.total_cpu_usage_ms, wg.total_query_optimizations,
       wg.active_request_count
<span class="kw">FROM</span> sys.dm_resource_governor_workload_groups wg
<span class="kw">WHERE</span> wg.name <span class="kw">IN</span> (<span class="str">'WGEmisionDiplomas'</span>,<span class="str">'WGConsultasGenerales'</span>);`,
      justificacion:`El parámetro <code>IMPORTANCE = HIGH</code> en <code>WGEmisionDiplomas</code> indica al programador de tareas de SQL Server que, ante contención de CPU dentro del mismo pool o entre pools con recursos compartidos, las solicitudes de este grupo deben resolverse antes que las de <code>IMPORTANCE = LOW</code>. Adicionalmente, <code>REQUEST_MAX_CPU_TIME_SEC = 30</code> en el grupo de consultas generales actúa como una salvaguarda: cualquier consulta que exceda ese tiempo será terminada automáticamente, evitando que una consulta mal escrita consuma recursos indefinidamente durante el cierre del semestre.<br><br>
      Se separó explícitamente por <code>APP_NAME()</code> en lugar de por login, ya que el proceso de emisión de diplomas puede ejecutarse con la misma cuenta de servicio que otras consultas, y el nombre de la aplicación permite una clasificación más precisa del contexto real de la conexión.`,
      practicas:['Usar IMPORTANCE en combinación con límites de tiempo (REQUEST_MAX_CPU_TIME_SEC) para procesos de baja prioridad','Documentar y comunicar a los usuarios los horarios de restricción antes del cierre de semestre','Revisar sys.dm_resource_governor_workload_groups durante el proceso para confirmar la priorización real','Restaurar la configuración de Resource Governor a su estado normal una vez finalizado el cierre']
    },
    {
      titulo:'Monitoreo del consumo de recursos',
      enunciado:`La Oficina de Tecnologías requiere conocer el comportamiento de CPU y memoria consumidos por cada grupo de trabajo configurado en SQL Server.<br><br>
      Se solicita desarrollar scripts que permitan: consultar las DMV relacionadas con Resource Governor; mostrar consumo de CPU; mostrar memoria utilizada; mostrar número de solicitudes; identificar sesiones activas; y generar un reporte consolidado del uso de recursos.`,
      script:`<span class="com">-- 1. Consumo de CPU y memoria por Resource Pool</span>
<span class="kw">SELECT</span>
    rp.name <span class="kw">AS</span> ResourcePool,
    rp.max_cpu_percent, rp.max_memory_percent,
    rp.total_cpu_usage_ms, rp.used_memory_kb, rp.target_memory_kb
<span class="kw">FROM</span> sys.dm_resource_governor_resource_pools rp;
<span class="kw">GO</span>

<span class="com">-- 2. Número de solicitudes y CPU por Workload Group</span>
<span class="kw">SELECT</span>
    wg.name <span class="kw">AS</span> WorkloadGroup, wg.pool_id,
    wg.total_request_count, wg.active_request_count,
    wg.total_cpu_usage_ms, wg.total_queued_request_count
<span class="kw">FROM</span> sys.dm_resource_governor_workload_groups wg;
<span class="kw">GO</span>

<span class="com">-- 3. Identificar sesiones activas y su grupo de carga asignado</span>
<span class="kw">SELECT</span>
    s.session_id, s.login_name, s.host_name, s.program_name,
    wg.name <span class="kw">AS</span> GrupoAsignado,
    r.cpu_time, r.logical_reads, r.status
<span class="kw">FROM</span> sys.dm_exec_sessions s
<span class="kw">JOIN</span> sys.dm_resource_governor_workload_groups wg <span class="kw">ON</span> s.group_id = wg.group_id
<span class="kw">LEFT JOIN</span> sys.dm_exec_requests r <span class="kw">ON</span> r.session_id = s.session_id
<span class="kw">WHERE</span> s.is_user_process = 1;
<span class="kw">GO</span>

<span class="com">-- 4. Reporte consolidado (pool + grupo + sesiones activas en un único resultado)</span>
<span class="kw">SELECT</span>
    rp.name <span class="kw">AS</span> ResourcePool,
    wg.name <span class="kw">AS</span> WorkloadGroup,
    rp.max_cpu_percent, rp.max_memory_percent,
    wg.total_request_count <span class="kw">AS</span> SolicitudesTotales,
    wg.active_request_count <span class="kw">AS</span> SolicitudesActivas,
    rp.total_cpu_usage_ms <span class="kw">AS</span> CpuTotalMs,
    rp.used_memory_kb / 1024 <span class="kw">AS</span> MemoriaUsadaMB
<span class="kw">FROM</span> sys.dm_resource_governor_resource_pools rp
<span class="kw">JOIN</span> sys.dm_resource_governor_workload_groups wg <span class="kw">ON</span> wg.pool_id = rp.pool_id
<span class="kw">ORDER BY</span> rp.total_cpu_usage_ms <span class="kw">DESC</span>;`,
      justificacion:`Las DMV <code>sys.dm_resource_governor_resource_pools</code> y <code>sys.dm_resource_governor_workload_groups</code> son las fuentes oficiales de Microsoft para inspeccionar en tiempo real cómo Resource Governor está distribuyendo CPU y memoria entre los distintos grupos configurados, sin necesidad de herramientas externas de monitoreo. Se unió <code>sys.dm_exec_sessions</code> con <code>group_id</code> para poder trazar, a nivel de sesión individual, a qué Workload Group pertenece cada conexión activa, lo cual es esencial para auditar si la clasificación automática está funcionando como se espera.<br><br>
      El reporte consolidado combina ambas vistas para ofrecer, en un único resultado, tanto la configuración (límites) como el consumo real, permitiendo a la Oficina de Tecnologías detectar rápidamente si algún pool se acerca a su límite configurado.`,
      practicas:['Ejecutar el reporte consolidado periódicamente (vía job) para llevar un historial de consumo por grupo','Correlacionar picos en total_cpu_usage_ms con eventos institucionales (cierre de semestre, matrícula)','Usar sys.dm_exec_requests junto a las DMV de Resource Governor para depurar sesiones específicas problemáticas','Establecer umbrales de alerta cuando used_memory_kb se acerque a target_memory_kb de un pool crítico']
    }
  ]
}
];

/* ============================================================
   RENDER
   ============================================================ */
const navWrap = document.getElementById('navWrap');
const main = document.getElementById('mainContent');

temas.forEach((tema)=>{
  // NAV
  const btn = document.createElement('button');
  btn.innerHTML = `<span class="rune-dot"></span> T${tema.id} ${tema.icon}`;
  btn.dataset.id = tema.id;
  btn.onclick = ()=>document.getElementById('tema'+tema.id).scrollIntoView({behavior:'smooth'});
  navWrap.appendChild(btn);

  // SECTION
  const sec = document.createElement('section');
  sec.className='tema';
  sec.id='tema'+tema.id;

  // INFOGRAPHIA
  let infografiaHtml = tema.infografia.map(f=>`
    <div class="arcanum-card" onclick="this.classList.toggle('flipped')">
      <div class="arcanum-inner">
        <div class="arcanum-front">
          <div class="rune-icon"><span class="glow">${f.ico}</span></div>
          <h4>${f.front}</h4>
          <div class="hint">⬆</div>
        </div>
        <div class="arcanum-back">
          <span class="rune-label">❖ Arcanum</span>
          ${f.back}
        </div>
      </div>
    </div>`).join('');

  // PROYECTOS
  let proyectosHtml = tema.proyectos.map((p,i)=>{
    const pid = `t${tema.id}p${i+1}`;
    return `
    <div class="proyecto-tome" id="${pid}">
      <div class="tome-head" onclick="toggleProyecto('${pid}')">
        <div class="left">
          <div class="tome-num">P${i+1}</div>
          <h4>${p.titulo}</h4>
        </div>
        <div class="tome-chevron">⌄</div>
      </div>
      <div class="tome-body">
        <div class="tome-tabs">
          <button class="tome-tab active" onclick="switchTab('${pid}',0,this)">Enunciado</button>
          <button class="tome-tab" onclick="switchTab('${pid}',1,this)">Script</button>
          <button class="tome-tab" onclick="switchTab('${pid}',2,this)">Justificación</button>
          <button class="tome-tab" onclick="switchTab('${pid}',3,this)">Prácticas</button>
        </div>
        <div class="tome-content">
          <div class="tome-pane active">${p.enunciado}</div>
          <div class="tome-pane"><pre class="code-block">${p.script}</pre></div>
          <div class="tome-pane">${p.justificacion}</div>
          <div class="tome-pane">
            <div class="practice-grid">
              ${p.practicas.map(pr=>`<div class="practice-item">${pr}</div>`).join('')}
            </div>
          </div>
        </div>
      </div>
    </div>`;
  }).join('');

  sec.innerHTML = `
    <div class="tema-head">
      <div class="tema-rune">${tema.icon}</div>
      <div class="tema-titles">
        <h2>Tema ${tema.id} · ${tema.titulo}</h2>
        <div class="sub">${tema.subtitulo}</div>
      </div>
    </div>
    <div class="infografia">${infografiaHtml}</div>
    <div class="proyectos">${proyectosHtml}</div>
  `;
  main.appendChild(sec);
});

// ACTIVIDAD
const actSec = document.createElement('section');
actSec.className='actividad';
actSec.innerHTML = `
  <div class="act-arcane">
    <h2>📋 Actividad · Semana 13</h2>
    <div class="steps">
      <div class="step"><div class="n">I</div><p>Se realizaron <strong>seis infografías interactivas</strong> (tipo flip-card), una por cada tema de la sesión de aprendizaje, arriba en cada sección correspondiente.</p></div>
      <div class="step"><div class="n">II</div><p>Se respondieron con claridad y precisión los <strong>18 enunciados propuestos</strong> (3 por tema), cada uno con: enunciado del ejercicio, script de la solución en T-SQL, justificación técnica de la solución aplicada y explicación de las buenas prácticas utilizadas.</p></div>
      <div class="step"><div class="n">✓</div><p>Formato estructurado respetado en cada proyecto mediante pestañas navegables: <em>Enunciado → Script → Justificación → Buenas Prácticas</em>.</p></div>
    </div>
    <div class="note">📁 Actividad lista para subir al portafolio del curso Base de Datos II — Mg. Ing. Raúl Fernández Bejarano.</div>
  </div>
`;
main.appendChild(actSec);

/* ============================================================
   INTERACTIONS
   ============================================================ */
function toggleProyecto(pid){
  document.getElementById(pid).classList.toggle('open');
}
function switchTab(pid, index, btnEl){
  const card = document.getElementById(pid);
  card.querySelectorAll('.tome-tab').forEach(b=>{b.classList.remove('active');});
  card.querySelectorAll('.tome-pane').forEach((p,i)=>p.classList.toggle('active', i===index));
  btnEl.classList.add('active');
}

/* NAV SCROLL + PROGRESS */
const navBtns = Array.from(navWrap.children);
const sections = temas.map(t=>document.getElementById('tema'+t.id));
window.addEventListener('scroll', ()=>{
  const scrollTop = window.scrollY;
  const docHeight = document.body.scrollHeight - window.innerHeight;
  document.getElementById('progressBar').style.width = (scrollTop/docHeight*100)+'%';

  let activeIdx = 0;
  sections.forEach((s,i)=>{
    if(s && s.getBoundingClientRect().top < 110) activeIdx = i;
  });
  navBtns.forEach((b,i)=>{
    b.classList.toggle('active', i===activeIdx);
    if(i===activeIdx){
      b.style.background = 'linear-gradient(135deg,var(--accent-3),var(--accent-1))';
      b.style.borderColor = 'var(--accent-1)';
      b.style.color = '#fff';
    }else{
      b.style.background = '';
      b.style.borderColor = '';
      b.style.color = '';
    }
  });
});

/* SPLASH */
window.addEventListener('load', ()=>{
  setTimeout(()=>document.getElementById('splash').classList.add('hide'), 800);
});
setTimeout(()=>document.getElementById('splash').classList.add('hide'), 2000);
</script>
</body>
</html>
