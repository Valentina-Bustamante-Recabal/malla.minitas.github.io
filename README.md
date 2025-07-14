<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Malla Interactiva – Ingeniería Civil en Minas - Universidad Central</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      background-color: #000;
      color: #fff;
    }
    h1 {
      text-align: center;
      margin-bottom: 20px;
    }
    .tabla-malla {
      display: grid;
      grid-template-columns: repeat(10, auto);
      gap: 20px;
      justify-content: center;
      margin-bottom: 20px;
    }
    .anio-label {
      grid-column: span 2;
      text-align: center;
      font-weight: bold;
      font-size: 18px;
      margin-bottom: -10px;
      background-color: #1a1a1a;
      padding: 10px;
      border-radius: 6px;
      border: 1px solid #444;
    }
    .semestre {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
    }
    .semestre h2 {
      font-size: 16px;
      margin-bottom: 10px;
    }
    .contenedor-ramo {
      display: flex;
      flex-direction: column;
      gap: 10px;
      align-items: center;
      position: relative;
    }
    .ramo {
      width: 100px;
      height: 100px;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      font-size: 12px;
      color: white;
      padding: 8px;
      box-sizing: border-box;
      cursor: pointer;
      position: relative;
      overflow: visible;
    }
    .ramo.tachado {
      background-color: #888 !important;
    }
    .ramo.tachado::after {
      content: "";
      position: absolute;
      top: 50%;
      left: 0;
      width: 100%;
      height: 2px;
      background-color: red;
      transform: rotate(-45deg);
      transform-origin: center;
    }
    .credito {
      position: absolute;
      top: -5px;
      right: -5px;
      width: 20px;
      height: 20px;
      background-color: #fff;
      color: #000;
      font-size: 10px;
      font-weight: bold;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 10;
    }
    .profesional {
      background-color: #FFA54F;
    }
    .disciplinar {
      background-color: #4F83FF;
    }
    .general {
      background-color: #C0C0C0;
      color: #000;
    }
    .barra-progreso {
      width: 100%;
      max-width: 600px;
      margin: 30px auto 20px;
    }
    .barra {
      height: 20px;
      background-color: #ddd;
      border-radius: 10px;
      overflow: hidden;
    }
    .progreso {
      height: 100%;
      background-color: #4caf50;
      text-align: center;
      color: white;
      line-height: 20px;
      transition: width 0.3s;
    }
    .boton-limpiar {
      display: block;
      margin: 0 auto 40px;
      padding: 10px 20px;
      background-color: #ff5252;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <h1>Malla Interactiva – Ingeniería Civil en Minas<br><span style='font-size: 18px;'>Universidad Central</span></h1>
  <div class="tabla-malla" id="malla"></div>
  <div class="barra-progreso">
    <div class="barra">
      <div id="progreso" class="progreso" style="width: 0%">0%</div>
    </div>
  </div>
  <button class="boton-limpiar" onclick="limpiarMalla()">Limpiar Malla</button>
  <div style="text-align:center; margin-top: -10px; font-size: 14px;" id="info-progreso">0 créditos completados de 300</div>
  <script>
    const estructura = [
      ["1er Año", [
        ["Semestre 1", 
          ["Introducción a las Matemáticas", "Introducción a la Física", "Introducción a la Ingeniería de Minas", "Formación Básica para la Vida Académica I", "Curso Sello Institucional I: Inglés I"], 
          ["disciplinar", "disciplinar", "disciplinar", "general", "general"], 
          [7, 7, 9, 4, 4],
          [["none"], ["none"], ["none"], ["none"], ["none"]]
        ],
        ["Semestre 2", 
          ["Álgebra I", "Cálculo I", "Mecánica","Formación Básica para la Vida Académica II", "Curso Sello Institucional II: Inglés II"], 
          ["disciplinar", "disciplinar", "disciplinar","general","general"], 
          [7, 7, 7, 4, 4],
          [["Introducción a las Matemáticas"], ["Admisión"], ["Introducción a la Física"], ["Admisión"], ["Curso Sello Institucional I: Inglés I"]]
        ]
      ]],
      ["2º Año", [
        ["Semestre 3", 
          ["Álgebra II", "Cálculo II", "Química General", "Geología General", "TIC para la Minería", "Curso Sello Institucional III"], 
          ["disciplinar", "disciplinar", "disciplinar", "disciplinar", "disciplinar", "general"], 
          [5, 5, 7, 5, 5, 4],
          [["Álgebra I"], ["Cálculo I"], ["Admisión"], ["Introducción a la Física"], ["Admisión"], ["Admisión"]]
        ],
        ["Semestre 4", 
          ["Ecuaciones Diferenciales", "Cálculo III", "Electricidad y Magnetismo", "Métodos de Producción Minera", "Petrografía y Mineralogía", "Curso Sello Institucional IV"], 
          ["disciplinar", "disciplinar", "disciplinar", "disciplinar", "disciplinar", "general"], 
          [5, 5, 5, 5, 5, 4],
          [["Álgebra II"], ["Cálculo II"], ["Admisión"], ["Admisión"], ["Geología General"], ["Admisión"]]
        ]
      ]],
      ["3er Año", [
        ["Semestre 5", 
          ["Probabilidad y Estadística", "Fundamentos de Economía", "Investigación Operativa", "Termodinámica", "Métodos Numéricos", "Topografía y Geomensura Minera"], 
          ["disciplinar", "disciplinar", "disciplinar", "disciplinar", "disciplinar", "disciplinar"], 
          [5, 4, 6, 5, 5, 5],
          [["Cálculo II"], ["Admisión"], ["Admisión"], ["Química General"], ["Admisión"], ["Métodos de Producción Minera"]]
        ],
        ["Semestre 6", 
          ["Geoestadística", "Evaluación de Proyectos", "Exploración y Geología Económica", "Perforación y Tronadura", "Taller Integrador I", "Interdisciplinar"], 
          ["disciplinar", "disciplinar", "disciplinar", "disciplinar", "disciplinar", "general"], 
          [5, 6, 5, 5, 5, 4],
          [["Probabilidad y Estadística"], ["Fundamentos de Economía"], ["Admisión"], ["Topografía y Geomensura Minera"], ["Introducción a las Matemáticas", "Introducción a la Física", "Introducción a la Ingeniería de Minas", "Formación Básica para la Vida Académica I", "Curso Sello Institucional I: Inglés I", "Álgebra I", "Cálculo I", "Mecánica", "Formación Básica para la Vida Académica II", "Curso Sello Institucional II: Inglés II", "Álgebra II", "Cálculo II", "Química General", "Geología General", "TIC para la Minería", "Curso Sello Institucional III", "Ecuaciones Diferenciales", "Cálculo III", "Electricidad y Magnetismo", "Métodos de Producción Minera", "Petrografía y Mineralogía", "Curso Sello Institucional IV"], ["Admisión"]]
        ]
      ]],
      ["4º Año", [
        ["Semestre 7", 
          ["Evaluación Económica de Yacimientos", "Mecánica de Fluidos", "Fundamentos de Metalurgia", "Carguío y Transporte", "Mecánica de Rocas", "Interdisciplinar II A+S", "Práctica Operacional"], 
          ["disciplinar", "disciplinar", "disciplinar", "disciplinar", "disciplinar", "general", "disciplinar"], 
          [4, 4, 4, 5, 4, 4, 7],
          [["Evaluación de Proyectos"], ["Admisión"], ["Admisión"], ["Perforación y Tronadura"], ["Admisión"], ["Admisión"], ["Introducción a las Matemáticas", "Introducción a la Física", "Introducción a la Ingeniería de Minas", "Formación Básica para la Vida Académica I", "Curso Sello Institucional I: Inglés I", "Álgebra I", "Cálculo I", "Mecánica", "Formación Básica para la Vida Académica II", "Curso Sello Institucional II: Inglés II", "Álgebra II", "Cálculo II", "Química General", "Geología General", "TIC para la Minería", "Curso Sello Institucional III", "Ecuaciones Diferenciales", "Cálculo III", "Electricidad y Magnetismo", "Métodos de Producción Minera", "Petrografía y Mineralogía", "Curso Sello Institucional IV", "Probabilidad y Estadística", "Fundamentos de Economía", "Investigación Operativa", "Termodinámica", "Métodos Numéricos", "Topografía y Geomensura Minera", "Geoestadística", "Evaluación de Proyectos", "Exploración y Geología Económica", "Perforación y Tronadura", "Taller Integrador I", "Interdisciplinar"]]
        ],
        ["Semestre 8", 
          ["Software de Productividad Minera", "Ventilación y Servicios Mina", "Procesamiento de Minerales", "Legislación Minera", "Seguridad y Medio Ambiente en Minería", "Taller Integrador II"], 
          ["disciplinar", "disciplinar", "disciplinar", "disciplinar", "disciplinar", "disciplinar"], 
          [6, 5, 5, 4, 4, 4],
          [["Evaluación Económica de Yacimientos"], ["Admisión"], ["Fundamentos de Metalurgia"], ["Exploración y Geología Económica"], ["Admisión"], ["Introducción a las Matemáticas", "Introducción a la Física", "Introducción a la Ingeniería de Minas", "Formación Básica para la Vida Académica I", "Curso Sello Institucional I: Inglés I", "Álgebra I", "Cálculo I", "Mecánica", "Formación Básica para la Vida Académica II", "Curso Sello Institucional II: Inglés II", "Álgebra II", "Cálculo II", "Química General", "Geología General", "TIC para la Minería", "Curso Sello Institucional III", "Ecuaciones Diferenciales", "Cálculo III", "Electricidad y Magnetismo", "Métodos de Producción Minera", "Petrografía y Mineralogía", "Curso Sello Institucional IV", "Probabilidad y Estadística", "Fundamentos de Economía", "Investigación Operativa", "Termodinámica", "Métodos Numéricos", "Topografía y Geomensura Minera", "Geoestadística", "Evaluación de Proyectos", "Exploración y Geología Económica", "Perforación y Tronadura", "Taller Integrador I", "Interdisciplinar", "Evaluación Económica de Yacimientos", "Mecánica de Fluidos", "Fundamentos de Metalurgia", "Carguío y Transporte", "Mecánica de Rocas", "Interdisciplinar II A+S", "Práctica Operacional"]]
        ]
      ]],
      ["5º Año", [
        ["Semestre 9", 
          ["Proyecto de Titulo I", "Gestión de Empresas y Liderazgo en Minería", "Diseño y Planificación Mina Subterránea", "Diseño y Planificación Rajo Abierto", "Excelencia Operacional"], 
          ["profesional", "profesional", "profesional", "profesional", "profesional"], 
          [7, 4, 6, 6, 6],
          [["Introducción a las Matemáticas", "Introducción a la Física", "Introducción a la Ingeniería de Minas", "Formación Básica para la Vida Académica I", "Curso Sello Institucional I: Inglés I", "Álgebra I", "Cálculo I", "Mecánica", "Formación Básica para la Vida Académica II", "Curso Sello Institucional II: Inglés II", "Álgebra II", "Cálculo II", "Química General", "Geología General", "TIC para la Minería", "Curso Sello Institucional III", "Ecuaciones Diferenciales", "Cálculo III", "Electricidad y Magnetismo", "Métodos de Producción Minera", "Petrografía y Mineralogía", "Curso Sello Institucional IV", "Probabilidad y Estadística", "Fundamentos de Economía", "Investigación Operativa", "Termodinámica", "Métodos Numéricos", "Topografía y Geomensura Minera", "Geoestadística", "Evaluación de Proyectos", "Exploración y Geología Económica", "Perforación y Tronadura", "Taller Integrador I", "Interdisciplinar", "Evaluación Económica de Yacimientos", "Mecánica de Fluidos", "Fundamentos de Metalurgia", "Carguío y Transporte", "Mecánica de Rocas", "Interdisciplinar II A+S", "Práctica Operacional", "Software de Productividad Minera", "Ventilación y Servicios Mina", "Procesamiento de Minerales", "Legislación Minera", "Seguridad y Medio Ambiente en Minería", "Taller Integrador II"], ["Software de Productividad Minera"], ["Ventilación y Servicios Mina"], ["Admisión"], ["Admisión"]]
        ],
        ["Semestre 10", 
          ["Proyecto de Título II", "Gestión de la Tecnología en Minería", "Responsabilidad Social Empresarial en Minería", "Innovación y Emprendimiento en Minería", "Práctica Profesional"], 
          ["profesional", "profesional", "profesional", "profesional", "profesional"], 
          [12, 4, 4, 4, 7],
          [["Proyecto de Título I"], ["Excelencia Operacional"], ["Seguridad y Medioambiente en Minería"], ["Admisión"], ["Introducción a las Matemáticas", "Introducción a la Física", "Introducción a la Ingeniería de Minas", "Formación Básica para la Vida Académica I", "Curso Sello Institucional I: Inglés I", "Álgebra I", "Cálculo I", "Mecánica", "Formación Básica para la Vida Académica II", "Curso Sello Institucional II: Inglés II", "Álgebra II", "Cálculo II", "Química General", "Geología General", "TIC para la Minería", "Curso Sello Institucional III", "Ecuaciones Diferenciales", "Cálculo III", "Electricidad y Magnetismo", "Métodos de Producción Minera", "Petrografía y Mineralogía", "Curso Sello Institucional IV", "Probabilidad y Estadística", "Fundamentos de Economía", "Investigación Operativa", "Termodinámica", "Métodos Numéricos", "Topografía y Geomensura Minera", "Geoestadística", "Evaluación de Proyectos", "Exploración y Geología Económica", "Perforación y Tronadura", "Taller Integrador I", "Interdisciplinar", "Evaluación Económica de Yacimientos", "Mecánica de Fluidos", "Fundamentos de Metalurgia", "Carguío y Transporte", "Mecánica de Rocas", "Interdisciplinar II A+S", "Práctica Operacional", "Software de Productividad Minera", "Ventilación y Servicios Mina", "Procesamiento de Minerales", "Legislación Minera", "Seguridad y Medio Ambiente en Minería", "Taller Integrador II", "Proyecto de Titulo I", "Gestión de Empresas y Liderazgo en Minería", "Diseño y Planificación Mina Subterránea", "Diseño y Planificación Rajo Abierto", "Excelencia Operacional"]]
        ]
      ]]
    ];

    function crearMalla() {
      const malla = document.getElementById("malla");
      estructura.forEach(([anio, semestres]) => {
        const label = document.createElement("div");
        label.className = "anio-label";
        label.textContent = anio;
        malla.appendChild(label);
      });
      estructura.forEach(([_, semestres]) => {
        semestres.forEach(([nombreSem, asignaturas, tipos, creditos, prereqs]) => {
          const divSem = document.createElement("div");
          divSem.className = "semestre";
          const titulo = document.createElement("h2");
          titulo.textContent = nombreSem;
          divSem.appendChild(titulo);
          const contenedor = document.createElement("div");
          contenedor.className = "contenedor-ramo";
          asignaturas.forEach((nombre, i) => {
            const tipo = tipos[i];
            const credito = creditos[i] || 6;
            const id = `${nombreSem}-${nombre}`;
            const ramo = document.createElement("div");
            ramo.className = `ramo ${tipo}`;
            ramo.innerHTML = `<div class="credito">${credito}</div>${nombre}`;
            ramo.setAttribute("data-prereq", JSON.stringify(prereqs[i]));
            ramo.setAttribute("data-nombre", nombre);
            if (localStorage.getItem(id) === "1") ramo.classList.add("tachado");
            ramo.onclick = () => {
              if (ramo.style.pointerEvents === "none") return;
              ramo.classList.toggle("tachado");
              localStorage.setItem(id, ramo.classList.contains("tachado") ? "1" : "0");
              habilitarRamos();
              actualizarProgreso();
            };
            contenedor.appendChild(ramo);
          });
          divSem.appendChild(contenedor);
          malla.appendChild(divSem);
        });
      });
      habilitarRamos();
      actualizarProgreso();
    }

    function actualizarProgreso() {
      let completadosCreditos = 0;
      const totalCreditos = 300;
      document.querySelectorAll(".ramo").forEach(ramo => {
        const credito = parseInt(ramo.querySelector(".credito").textContent);
        if (!isNaN(credito) && ramo.classList.contains("tachado")) {
          completadosCreditos += credito;
        }
      });
      const porcentaje = Math.round((completadosCreditos / totalCreditos) * 100);
      const barra = document.getElementById("progreso");
      barra.style.width = `${porcentaje}%`;
      barra.textContent = `${porcentaje}%`;
      document.getElementById("info-progreso").textContent = `${completadosCreditos} créditos completados de ${totalCreditos}`;
    }

    function limpiarMalla() {
      document.querySelectorAll(".ramo.tachado").forEach(r => {
        r.classList.remove("tachado");
        const id = `${r.parentElement.previousSibling.textContent}-${r.getAttribute("data-nombre")}`;
        localStorage.removeItem(id);
      });
      habilitarRamos();
      actualizarProgreso();
    }

    function habilitarRamos() {
      document.querySelectorAll(".ramo").forEach(ramo => {
        const prereqRaw = ramo.getAttribute("data-prereq");
        const prereqs = JSON.parse(prereqRaw);

        // Si es "none" o solo "Admisión", se habilita directamente
        if (prereqs.length === 1 && (prereqs[0] === "none" || prereqs[0] === "Admisión")) {
          ramo.style.opacity = "1";
          ramo.style.pointerEvents = "auto";
          ramo.title = "";
        } else {
          let habilitado = true;
          for (let p of prereqs) {
            if (p === "Admisión") continue;

            const ramoPrereq = Array.from(document.querySelectorAll(".ramo")).find(r => {
              return r.getAttribute("data-nombre")?.trim().toLowerCase() === p.trim().toLowerCase()
                && r.classList.contains("tachado");
            });

            if (!ramoPrereq) {
              habilitado = false;
              break;
            }
          }

          ramo.style.opacity = habilitado ? "1" : "0.5";
          ramo.style.pointerEvents = habilitado ? "auto" : "none";
          ramo.title = habilitado ? "" : "Debes aprobar los prerrequisitos para habilitar este ramo.";
        }
      });
    }

    window.onload = crearMalla;
  </script>
  <!-- Mensaje informativo al inicio -->
<div id="mensaje-informativo" style="
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background-color: rgba(0, 0, 0, 0.85);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  color: white;
  font-size: 16px;
  text-align: center;
  padding: 20px;
  cursor: pointer;
">
  <div>
    <p>El número en la esquina superior derecha indica la cantidad de créditos del ramo.</p>
    <p>Los ramos visibles son aquellos cuyo único requisito es "Admisión".</p>
    <p style="margin-top: 20px;">Haz clic en cualquier lado para comenzar.</p>
    <p style="margin-top: 20px;">------------------------</p>
    <p style="margin-top: 20px;">Creado por Valentina Bustamante Recabal :)</p>
  </div>
</div>

<script>
  // Elimina el mensaje al hacer clic en cualquier parte de la pantalla
  document.addEventListener("click", function() {
    const mensaje = document.getElementById("mensaje-informativo");
    if (mensaje) {
      mensaje.remove();
    }
  });
</script>
</body>
</html>
