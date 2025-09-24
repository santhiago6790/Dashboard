<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard Macroeconómico</title>
    <!-- Tailwind CSS desde CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Configuración para usar la fuente Inter -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
    </style>
    <!-- Chart.js para los gráficos -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body class="bg-gray-900 text-gray-100">
    <div class="container mx-auto p-4 md:p-8">
        <h1 class="text-3xl md:text-5xl font-extrabold text-center text-white mb-2">Dashboard Temático</h1>
        <p class="text-lg text-center text-gray-400 mb-8 md:mb-12">Datos Clave de la Región</p>

        <!-- Sección de Entorno Macroeconómico -->
        <h2 class="text-3xl font-bold text-center text-white mb-6">Entorno Macroeconómico</h2>
        <div id="macro-data-container" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mb-12">
            <!-- Las tarjetas se generarán aquí con JavaScript -->
        </div>
        
        <!-- Gráficos para el año 2024 -->
        <h3 class="text-2xl font-bold text-center text-gray-300 mb-6">Datos 2024</h3>
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-12">
            <div class="bg-gray-800 rounded-xl shadow-lg p-6 md:p-8">
                <h4 class="text-xl font-bold mb-4 text-center">Crecimiento del PIB 2024</h4>
                <div class="w-full h-80">
                    <canvas id="pibChart2024"></canvas>
                </div>
            </div>
            <div class="bg-gray-800 rounded-xl shadow-lg p-6 md:p-8">
                <h4 class="text-xl font-bold mb-4 text-center">Inflación dic/2024</h4>
                <div class="w-full h-80">
                    <canvas id="inflationChart2024"></canvas>
                </div>
            </div>
            <div class="bg-gray-800 rounded-xl shadow-lg p-6 md:p-8">
                <h4 class="text-xl font-bold mb-4 text-center">Tasa de Interés dic/2024</h4>
                <div class="w-full h-80">
                    <canvas id="interestRateChart2024"></canvas>
                </div>
            </div>
        </div>

        <!-- Gráficos para el año 2025 -->
        <h3 class="text-2xl font-bold text-center text-gray-300 mb-6">Proyecciones 2025</h3>
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-12">
            <div class="bg-gray-800 rounded-xl shadow-lg p-6 md:p-8">
                <h4 class="text-xl font-bold mb-4 text-center">Crecimiento del PIB 2025</h4>
                <div class="w-full h-80">
                    <canvas id="pibChart2025"></canvas>
                </div>
            </div>
            <div class="bg-gray-800 rounded-xl shadow-lg p-6 md:p-8">
                <h4 class="text-xl font-bold mb-4 text-center">Inflación proyectada 2025</h4>
                <div class="w-full h-80">
                    <canvas id="inflationChart2025"></canvas>
                </div>
            </div>
        </div>

        <!-- Sección de Transformación Digital -->
        <h2 class="text-3xl font-bold text-center text-white mb-6">Transformación Digital</h2>
        <div id="digital-data-container" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mb-12">
            <!-- Las tarjetas se generarán aquí con JavaScript -->
        </div>

        <!-- Gráficos de Transformación Digital -->
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-12">
            <div class="bg-gray-800 rounded-xl shadow-lg p-6 md:p-8">
                <h4 class="text-xl font-bold mb-4 text-center">Cobertura 4G/5G</h4>
                <div class="w-full h-80">
                    <canvas id="coverageChart"></canvas>
                </div>
            </div>
        </div>

        <!-- Sección de Sostenibilidad Ambiental -->
        <h2 class="text-3xl font-bold text-center text-white mb-6">Sostenibilidad Ambiental</h2>
        <div id="sustain-data-container" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mb-12">
            <!-- Las tarjetas se generarán aquí con JavaScript -->
        </div>

        <!-- Gráficos de Sostenibilidad Ambiental -->
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-12">
            <div class="bg-gray-800 rounded-xl shadow-lg p-6 md:p-8">
                <h4 class="text-xl font-bold mb-4 text-center">Huella Ecológica</h4>
                <div class="w-full h-80">
                    <canvas id="ecologicalFootprintChart"></canvas>
                </div>
            </div>
        </div>
    </div>

    <script>
        const entorno_macro = {
            "Colombia": {
                "Crecimiento PIB 2024": "1.8%", 
                "Inflación dic/2024": "5.2%", 
                "Tasa interés dic/2024": "9.50%",
                "Desempleo 2024": "10.5%",
                "Crecimiento PIB 2025": "2.7%",
                "Inflación proyectada 2025": "4.8%",
                "Tasa interés proyectada": "8.75%",
                "Desempleo proyectado": "9.8%"
            },
            "Panamá": {
                "Crecimiento PIB 2024": "2.5%", 
                "Inflación dic/2024": "0.2%", 
                "Estabilidad": "Alta",
                "Crecimiento PIB 2025": "3.6%",
                "Inflación proyectada": "1.1%",
                "Riesgo político": "Bajo"
            },
            "Centroamérica promedio": {
                "Crecimiento 2024": "2.8%", 
                "Riesgo político": "Moderado",
                "Crecimiento 2025": "1.0%",
                "Vulnerabilidad externa": "Alta",
                "Dependencia de EE.UU.": "Elevada"
            }
        };

        const transformacion_digital = {
            "Colombia": {
                "Madurez digital": "Media-Alta",
                "Cobertura 4G/5G": "85%",
                "Ley de protección de datos": "Ley 1581 de 2012",
                "Sandbox fintech": "Activo desde 2021",
                "Brecha digital": "Persistente en zonas rurales",
                "Adopción IA en banca": "+30% entidades con pilotos",
                "Educación superior": "Avance en aulas híbridas y campus virtuales"
            },
            "Panamá": {
                "Madurez digital": "Alta",
                "Cobertura 4G/5G": "90%",
                "Ley de protección de datos": "Ley 81 de 2019",
                "Regulación fintech": "Flexible y pro-inversión",
                "Educación superior": "Integración de plataformas LMS y credenciales digitales"
            },
            "Centroamérica promedio": {
                "Madurez digital": "Media",
                "Brecha digital": "Alta",
                "Regulación tecnológica": "Fragmentada",
                "Educación superior": "Desigual acceso a conectividad y recursos"
            }
        };

        const sostenibilidad_ambiental = {
            "Colombia": {
                "Riesgo climático": "Alto (El Niño/La Niña)",
                "Política climática": "Plan Nacional de Adaptación 2023-2030",
                "Inversión renovable": "↑ Energía solar y eólica",
                "Huella ecológica": "3.1 ha/persona",
                "Educación superior": "Incorporación de sostenibilidad en currículo"
            },
            "Panamá": {
                "Riesgo climático": "Moderado",
                "Política climática": "Estrategia Nacional de Cambio Climático",
                "Inversión renovable": "↑ Hidroeléctrica y solar",
                "Huella ecológica": "2.6 ha/persona",
                "Educación superior": "Programas en gestión ambiental y resiliencia"
            },
            "Centroamérica promedio": {
                "Riesgo climático": "Alto",
                "Vulnerabilidad": "Sequías, huracanes, deforestación",
                "Educación superior": "Limitada integración curricular"
            }
        };

        const macroDataContainer = document.getElementById('macro-data-container');
        const digitalDataContainer = document.getElementById('digital-data-container');
        const sustainDataContainer = document.getElementById('sustain-data-container');
        const pibChart2024Canvas = document.getElementById('pibChart2024');
        const inflationChart2024Canvas = document.getElementById('inflationChart2024');
        const interestRateChart2024Canvas = document.getElementById('interestRateChart2024');
        const pibChart2025Canvas = document.getElementById('pibChart2025');
        const inflationChart2025Canvas = document.getElementById('inflationChart2025');
        const coverageChartCanvas = document.getElementById('coverageChart');
        const ecologicalFootprintChartCanvas = document.getElementById('ecologicalFootprintChart');

        // Función para generar una tarjeta de datos
        function createCard(country, data) {
            const card = document.createElement('div');
            card.className = "bg-gray-800 rounded-xl shadow-lg p-6 hover:shadow-xl transition-shadow duration-300";

            const title = document.createElement('h3');
            title.className = "text-xl font-bold mb-4 text-white";
            title.textContent = country;
            card.appendChild(title);

            const ul = document.createElement('ul');
            ul.className = "space-y-2";
            for (const key in data) {
                const li = document.createElement('li');
                li.className = "flex justify-between items-center";
                
                const spanKey = document.createElement('span');
                spanKey.className = "text-gray-400";
                spanKey.textContent = key;
                
                const spanValue = document.createElement('span');
                spanValue.className = "font-semibold text-gray-200";
                spanValue.textContent = data[key];

                li.appendChild(spanKey);
                li.appendChild(spanValue);
                ul.appendChild(li);
            }
            card.appendChild(ul);

            return card;
        }

        // Función para preparar y renderizar un gráfico de barras
        function renderChart(canvas, chartTitle, dataObject, dataKeys, chartColor, labelSuffix = '%') {
            const chartData = {
                labels: [],
                values: [],
            };

            for (const country in dataObject) {
                let foundKey = dataKeys.find(key => dataObject[country].hasOwnProperty(key));
                if (foundKey) {
                    const value = parseFloat(dataObject[country][foundKey].replace('%', '').replace('+', '').replace(',', '.').trim());
                    if (!isNaN(value)) {
                        chartData.labels.push(country);
                        chartData.values.push(value);
                    }
                }
            }
            
            if (chartData.labels.length > 0) {
                 new Chart(canvas, {
                    type: 'bar',
                    data: {
                        labels: chartData.labels,
                        datasets: [{
                            label: chartTitle,
                            data: chartData.values,
                            backgroundColor: chartColor,
                            borderColor: chartColor,
                            borderWidth: 1,
                            borderRadius: 8,
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        scales: {
                            y: {
                                beginAtZero: true,
                                title: {
                                    display: true,
                                    text: 'Porcentaje (%)',
                                    color: 'white'
                                },
                                ticks: {
                                    color: 'white'
                                },
                                grid: {
                                    color: 'rgba(255, 255, 255, 0.1)'
                                }
                            },
                            x: {
                                title: {
                                    display: true,
                                    text: 'País / Región',
                                    color: 'white'
                                },
                                ticks: {
                                    color: 'white'
                                },
                                grid: {
                                    color: 'rgba(255, 255, 255, 0.1)'
                                }
                            }
                        },
                        plugins: {
                            legend: {
                                display: false
                            },
                            tooltip: {
                                callbacks: {
                                    label: function(context) {
                                        let label = context.dataset.label || '';
                                        if (label) {
                                            label += ': ';
                                        }
                                        if (context.parsed.y !== null) {
                                            label += context.parsed.y + labelSuffix;
                                        }
                                        return label;
                                    }
                                }
                            }
                        }
                    }
                });
            } else {
                const parentDiv = canvas.parentElement;
                parentDiv.innerHTML = <p class="text-center text-gray-500">Datos no disponibles para este gráfico.</p>;
            }
        }

        // Itera sobre los datos y crea las tarjetas para cada sección
        for (const country in entorno_macro) {
            const card = createCard(country, entorno_macro[country]);
            macroDataContainer.appendChild(card);
        }
        for (const country in transformacion_digital) {
            const card = createCard(country, transformacion_digital[country]);
            digitalDataContainer.appendChild(card);
        }
        for (const country in sostenibilidad_ambiental) {
            const card = createCard(country, sostenibilidad_ambiental[country]);
            sustainDataContainer.appendChild(card);
        }
       
        // Llama a la función para cada gráfico, usando un array de posibles claves
        renderChart(pibChart2024Canvas, 'Crecimiento del PIB 2024 (%)', entorno_macro, ['Crecimiento PIB 2024', 'Crecimiento 2024'], '#60A5FA'); // blue-400
        renderChart(inflationChart2024Canvas, 'Inflación dic/2024 (%)', entorno_macro, ['Inflación dic/2024'], '#F87171'); // red-400
        renderChart(interestRateChart2024Canvas, 'Tasa de Interés dic/2024 (%)', entorno_macro, ['Tasa interés dic/2024'], '#A78BFA'); // violet-400
        renderChart(pibChart2025Canvas, 'Crecimiento del PIB 2025 (%)', entorno_macro, ['Crecimiento PIB 2025', 'Crecimiento 2025'], '#34D399'); // emerald-400
        renderChart(inflationChart2025Canvas, 'Inflación proyectada 2025 (%)', entorno_macro, ['Inflación proyectada 2025', 'Inflación proyectada'], '#FBBF24'); // amber-400
        renderChart(coverageChartCanvas, 'Cobertura 4G/5G (%)', transformacion_digital, ['Cobertura 4G/5G'], '#818CF8', ' %'); // indigo-400
        renderChart(ecologicalFootprintChartCanvas, 'Huella Ecológica (ha/persona)', sostenibilidad_ambiental, ['Huella ecológica'], '#2DD4BF', ' ha/persona'); // teal-400
    </script>
</body>
</html>
