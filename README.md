<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
    <title>Calculadora Solar · Consumo + HSP · Móvil</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        }
        body {
            background: #e6eff8;
            margin: 0;
            padding: 10px;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: flex-start;
        }
        .container {
            max-width: 100%;
            width: 100%;
            background: white;
            border-radius: 1.5rem;
            box-shadow: 0 15px 30px -10px #0b2c44;
            padding: 1.2rem;
        }
        h1 {
            margin: 0 0 0.2rem 0;
            color: #1e3a5f;
            font-weight: 700;
            font-size: 1.8rem;
            border-left: 6px solid #f0b86e;
            padding-left: 0.8rem;
        }
        .subtitulo {
            color: #2b4b6f;
            font-size: 0.9rem;
            margin: 0.2rem 0 0.2rem 0;
        }
        .credito-sub {
            color: #1e3a5f;
            font-size: 0.8rem;
            font-weight: 500;
            margin-bottom: 1rem;
            border-bottom: 1px dashed #b5ccdf;
            padding-bottom: 0.5rem;
        }
        /* Calculadora grid */
        .calculadora-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 1rem;
            margin: 1.2rem 0;
        }
        @media (min-width: 640px) {
            .calculadora-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
        @media (min-width: 1024px) {
            .calculadora-grid {
                grid-template-columns: repeat(3, 1fr);
            }
        }
        .artefacto {
            background: #f9fbfd;
            border-radius: 1.2rem;
            padding: 1rem;
            border: 1px solid #d9e4f0;
        }
        .artefacto h2 {
            font-size: 1.1rem;
            margin: 0 0 0.8rem 0;
            color: #1e3a5f;
            display: flex;
            align-items: center;
            gap: 6px;
        }
        .artefacto h2::before {
            content: "⚡";
            font-size: 1.2rem;
        }
        .campo {
            margin-bottom: 0.6rem;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            gap: 0.3rem;
        }
        .campo label {
            font-weight: 500;
            color: #2b4b6f;
            min-width: 60px;
            font-size: 0.8rem;
        }
        .campo input, .campo select {
            flex: 1;
            min-width: 80px;
            padding: 0.5rem 0.6rem;
            border: 1px solid #b5ccdf;
            border-radius: 2rem;
            font-size: 0.9rem;
            background: white;
        }
        .nota {
            font-size: 0.7rem;
            color: #4d6a86;
            background: #e3edf6;
            padding: 0.2rem 0.8rem;
            border-radius: 30px;
            display: inline-block;
            margin: 0.2rem 0 0.5rem 0;
        }
        .btn-group {
            display: flex;
            flex-direction: column;
            gap: 0.8rem;
            margin: 1.2rem 0;
        }
        @media (min-width: 480px) {
            .btn-group {
                flex-direction: row;
                justify-content: center;
            }
        }
        .btn-calcular, .btn-reset {
            border: none;
            color: white;
            font-size: 1.1rem;
            font-weight: 600;
            padding: 0.8rem 1.5rem;
            border-radius: 3rem;
            cursor: pointer;
            transition: all 0.2s;
            box-shadow: 0 4px 8px rgba(0,20,40,0.2);
            flex: 1;
            max-width: 300px;
            margin: 0 auto;
            width: 100%;
        }
        .btn-calcular {
            background: #1e3a5f;
            border-bottom: 4px solid #0f263b;
        }
        .btn-reset {
            background: #6c8da8;
            border-bottom: 4px solid #4a627a;
        }
        .btn-calcular:active, .btn-reset:active {
            transform: translateY(2px);
            border-bottom-width: 2px;
        }
        .resultados {
            background: linear-gradient(145deg, #e2edf7, #ffffff);
            border-radius: 1.5rem;
            padding: 1.2rem;
            margin: 1.2rem 0;
            border: 2px solid #aac7df;
        }
        .fila-total {
            display: flex;
            align-items: center;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 0.5rem;
            margin: 0.5rem 0;
        }
        .total-label {
            font-size: 1rem;
            font-weight: 650;
            color: #0b2c44;
        }
        .total-valor {
            font-size: 1.6rem;
            font-weight: 800;
            background: #0b2c44;
            color: white;
            padding: 0.1rem 1.2rem;
            border-radius: 3rem;
            min-width: 120px;
            text-align: center;
        }
        .hsp-badge {
            background: #f0b86e;
            color: #1e3a5f;
            font-weight: 700;
            padding: 0.2rem 1rem;
            border-radius: 2rem;
            font-size: 0.8rem;
        }
        /* Panel HSP (simplificado, sin tabla) */
        .panel-hsp {
            background: #f9fbfe;
            border-radius: 1.2rem;
            padding: 1rem;
            border: 1px solid #d9e4f0;
            margin: 1.2rem 0;
        }
        .filtros {
            display: flex;
            flex-direction: column;
            gap: 0.8rem;
            margin-bottom: 1rem;
        }
        @media (min-width: 640px) {
            .filtros {
                flex-direction: row;
                flex-wrap: wrap;
            }
        }
        .filtros select, .filtros input {
            padding: 0.6rem 1rem;
            border-radius: 3rem;
            border: 1px solid #b5ccdf;
            font-size: 0.9rem;
            width: 100%;
        }
        .hsp-selected {
            background: #f0b86e30;
            border-radius: 1.5rem;
            padding: 0.8rem 1rem;
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
            margin-bottom: 0; /* ya no hay tabla debajo */
        }
        @media (min-width: 480px) {
            .hsp-selected {
                flex-direction: row;
                justify-content: space-between;
            }
        }
        /* Ocultamos la tabla y las estadísticas */
        .tabla-scroll, .card-stats {
            display: none;
        }
        .footer {
            text-align: center;
            color: #456f94;
            font-size: 0.7rem;
            margin-top: 1rem;
            border-top: 1px dashed #b5ccdf;
            padding-top: 0.8rem;
        }
        .credito {
            color: #1e3a5f;
            font-weight: 500;
            margin-top: 0.3rem;
        }
    </style>
</head>
<body>
<div class="container">
    <h1>☀️ Calculadora Solar</h1>
    <div class="subtitulo">Consumo + HSP Venezuela</div>
    <!-- Crédito debajo del subtítulo -->
    <div class="credito-sub">✧ Diseñada por Anderson Da Silva ✧</div>

    <!-- Formulario de consumo -->
    <form id="consumoForm" onsubmit="event.preventDefault(); calcularConsumo();">
        <div class="calculadora-grid">
            <!-- Lámparas LED -->
            <div class="artefacto">
                <h2>💡 Lámparas LED</h2>
                <div class="campo"><label>Watts:</label><input type="number" id="ledWatts" value="15" min="0" step="0.1"></div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="ledCant" value="1" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="ledHoras" value="1" min="0" step="0.1"></div>
            </div>

            <!-- REFLECTORES LED -->
            <div class="artefacto">
                <h2>💡 REFLECTORES LED</h2>
                <div class="campo"><label>Potencia:</label>
                    <select id="reflejoTipo">
                        <option value="50">50 W</option>
                        <option value="100">100 W</option>
                        <option value="200">200 W</option>
                    </select>
                </div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="reflejoCant" value="0" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="reflejoHoras" value="0" min="0" step="0.1"></div>
            </div>

            <!-- PUNTOS DE VENTA -->
            <div class="artefacto">
                <h2>💳 PUNTOS DE VENTA</h2>
                <div class="campo"><label>Watts fijo:</label><input type="number" id="puntoWatts" value="15" readonly style="background:#eef2f7;"></div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="puntoCant" value="0" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="puntoHoras" value="0" min="0" step="0.1"></div>
            </div>

            <!-- COMPUTADORAS -->
            <div class="artefacto">
                <h2>💻 COMPUTADORAS</h2>
                <div class="campo"><label>Tipo:</label>
                    <select id="pcTipo">
                        <option value="70">Laptop (70 W)</option>
                        <option value="140">Escritorio (140 W)</option>
                    </select>
                </div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="pcCant" value="0" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="pcHoras" value="0" min="0" step="0.1"></div>
            </div>

            <!-- IMPRESORAS -->
            <div class="artefacto">
                <h2>🖨️ IMPRESORAS</h2>
                <div class="campo"><label>Watts fijo:</label><input type="number" id="impresoraWatts" value="100" readonly style="background:#eef2f7;"></div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="impresoraCant" value="0" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="impresoraHoras" value="0" min="0" step="0.1"></div>
            </div>

            <!-- Televisor + receptor -->
            <div class="artefacto">
                <h2>📺 TV + receptor</h2>
                <div class="campo"><label>Tipo:</label>
                    <select id="tvTipo"><option value="50">42" (50 W)</option><option value="80">55" (80 W)</option></select>
                </div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="tvCant" value="1" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="tvHoras" value="2" min="0" step="0.1"></div>
            </div>

            <!-- Ventiladores -->
            <div class="artefacto">
                <h2>🌀 Ventiladores</h2>
                <div class="campo"><label>Tipo:</label>
                    <select id="ventTipo"><option value="50">Mediano (50 W)</option><option value="80">Grande (80 W)</option></select>
                </div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="ventCant" value="1" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="ventHoras" value="3" min="0" step="0.1"></div>
            </div>

            <!-- Bomba de agua -->
            <div class="artefacto">
                <h2>💧 Bomba agua</h2>
                <div class="campo"><label>Tipo:</label>
                    <select id="bombaTipo"><option value="400">1/2 HP (400 W)</option><option value="800">1 HP (800 W)</option></select>
                </div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="bombaCant" value="1" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="bombaHoras" value="1" min="0" step="0.1"></div>
            </div>

            <!-- Refrigerador -->
            <div class="artefacto">
                <h2>❄️ Refrigerador</h2>
                <div class="nota">Diario/Instantáneo</div>
                <div class="campo"><label>Tipo:</label>
                    <select id="refriTipo">
                        <option value="1000|120">100 L (1000 W/día · 120 W inst)</option>
                        <option value="1500|150">150 L (1500 W/día · 150 W inst)</option>
                        <option value="2500|180">300 L (2500 W/día · 180 W inst)</option>
                    </select>
                </div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="refriCant" value="1" min="0" step="1"></div>
            </div>

            <!-- Cargadores -->
            <div class="artefacto">
                <h2>📱 Cargadores</h2>
                <div class="campo"><label>Watts:</label><input type="number" id="cargWatts" value="10" min="0" step="0.1"></div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="cargCant" value="1" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="cargHoras" value="2" min="0" step="0.1"></div>
            </div>

            <!-- Lavadora -->
            <div class="artefacto">
                <h2>🧺 Lavadora</h2>
                <div class="campo"><label>Watts:</label><input type="number" id="lavaWatts" value="350" min="0" step="0.1"></div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="lavaCant" value="1" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="lavaHoras" value="1" min="0" step="0.1"></div>
            </div>

            <!-- Starlink -->
            <div class="artefacto">
                <h2>🛰️ Starlink</h2>
                <div class="campo"><label>Watts:</label><input type="number" id="starWatts" value="50" min="0" step="0.1"></div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="starCant" value="1" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="starHoras" value="24" min="0" step="0.1"></div>
            </div>

            <!-- Licuadora -->
            <div class="artefacto">
                <h2>🥤 Licuadora</h2>
                <div class="campo"><label>Watts:</label><input type="number" id="licuWatts" value="500" min="0" step="0.1"></div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="licuCant" value="1" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="licuHoras" value="0.5" min="0" step="0.1"></div>
            </div>

            <!-- Aire Acondicionado -->
            <div class="artefacto">
                <h2>❄️ AIRE ACONDICIONADO</h2>
                <div class="nota">Ciclo 50%</div>
                <div class="campo"><label>BTU:</label>
                    <select id="aireBTU">
                        <option value="5000">5.000 BTU (500 W)</option>
                        <option value="8000">8.000 BTU (800 W)</option>
                        <option value="12000">12.000 BTU (1200 W)</option>
                        <option value="18000">18.000 BTU (1800 W)</option>
                    </select>
                </div>
                <div class="campo"><label>Horas:</label>
                    <select id="aireHoras">
                        <option value="24">24 horas</option>
                        <option value="12">12 horas</option>
                        <option value="6">6 horas</option>
                        <option value="4">4 horas</option>
                    </select>
                </div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="aireCant" value="1" min="0" step="1"></div>
            </div>

            <!-- Router -->
            <div class="artefacto">
                <h2>📶 Router</h2>
                <div class="campo"><label>Watts fijo:</label><input type="number" id="routerWatts" value="12" readonly style="background:#eef2f7;"></div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="routerCant" value="1" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="routerHoras" value="24" min="0" step="0.1"></div>
            </div>

            <!-- OTROS 1 -->
            <div class="artefacto">
                <h2>🔌 OTROS 1</h2>
                <div class="campo"><label>Watts:</label><input type="number" id="otros1Watts" value="0" min="0" step="0.1"></div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="otros1Cant" value="0" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="otros1Horas" value="0" min="0" step="0.1"></div>
            </div>

            <!-- OTROS 2 -->
            <div class="artefacto">
                <h2>🔌 OTROS 2</h2>
                <div class="campo"><label>Watts:</label><input type="number" id="otros2Watts" value="0" min="0" step="0.1"></div>
                <div class="campo"><label>Cantidad:</label><input type="number" id="otros2Cant" value="0" min="0" step="1"></div>
                <div class="campo"><label>Horas/día:</label><input type="number" id="otros2Horas" value="0" min="0" step="0.1"></div>
            </div>
        </div>

        <!-- Botones -->
        <div class="btn-group">
            <button type="submit" class="btn-calcular">Calcular consumos</button>
            <button type="button" class="btn-reset" id="btnNuevoCalculo">⟲ Nuevo cálculo</button>
        </div>
    </form>

    <!-- Resultados consumo -->
    <div class="resultados">
        <div class="fila-total">
            <span class="total-label">⚡ Diario:</span>
            <span class="total-valor" id="resultadoDiario">0</span>
            <span style="font-size: 1.2rem;">Wh</span>
        </div>
        <div class="fila-total">
            <span class="total-label">⚡ Instantáneo:</span>
            <span class="total-valor" id="resultadoInstantaneo">0</span>
            <span style="font-size: 1.2rem;">W</span>
        </div>
    </div>

    <!-- Panel HSP (sin tabla) -->
    <div class="panel-hsp">
        <h2 style="color:#1e3a5f; margin-top:0; font-size:1.2rem;">🌞 Selecciona ciudad</h2>
        <div class="filtros">
            <select id="estadoFiltro"><option value="TODOS">Todos los estados</option></select>
            <select id="ciudadFiltro"><option value="TODAS">Todas las ciudades</option></select>
            <input type="text" id="busqueda" placeholder="Buscar...">
        </div>
        <div class="hsp-selected">
            <span><strong>HSP:</strong> <span id="hspValor">5.0</span> h</span>
            <span><strong>Ciudad:</strong> <span id="ciudadSeleccionada">Caracas</span></span>
        </div>
        <!-- Tabla oculta (no se muestra) -->
        <div class="tabla-scroll" style="display:none;">
            <!-- contenido eliminado visualmente -->
        </div>
        <div class="card-stats" style="display:none;"></div>
    </div>

    <!-- Resultado paneles -->
    <div class="resultados" style="background:#fff4e0; border-color:#f0b86e;">
        <div class="fila-total">
            <span class="total-label">☀️ Paneles necesarios:</span>
            <span class="total-valor" id="resultadoPaneles">0</span>
            <span style="font-size:1.2rem;">Wp</span>
        </div>
        <div class="fila-total">
            <span class="total-label">HSP = <span id="hspActual">5.0</span> h</span>
        </div>
    </div>

    <!-- Footer (se mantiene por si acaso, pero ya tenemos crédito arriba) -->
    <div class="footer">
        Datos referenciales · Venezuela 2025
    </div>
</div>

<script>
    (function() {
        // ------------------------------------------------------------
        // DATOS DE VENEZUELA (HSP) - mismos datos
        // ------------------------------------------------------------
        const datosVenezuela = [
            { estado: "Distrito Capital", municipio: "Libertador", ciudad: "Caracas", hsp: 5.0, zona: "Centro-Norte" },
            { estado: "Miranda", municipio: "Sucre", ciudad: "Petare", hsp: 5.1, zona: "Centro-Norte" },
            { estado: "Miranda", municipio: "Baruta", ciudad: "Baruta", hsp: 5.1, zona: "Centro-Norte" },
            { estado: "Miranda", municipio: "Chacao", ciudad: "Chacao", hsp: 5.1, zona: "Centro-Norte" },
            { estado: "Miranda", municipio: "Plaza", ciudad: "Guarenas", hsp: 5.2, zona: "Centro-Norte" },
            { estado: "Zulia", municipio: "Maracaibo", ciudad: "Maracaibo", hsp: 5.8, zona: "Costa-Lago" },
            { estado: "Zulia", municipio: "San Francisco", ciudad: "San Francisco", hsp: 5.8, zona: "Costa-Lago" },
            { estado: "Zulia", municipio: "Cabimas", ciudad: "Cabimas", hsp: 5.7, zona: "Costa" },
            { estado: "Zulia", municipio: "Colón", ciudad: "San Carlos del Zulia", hsp: 5.4, zona: "Sur del Lago" },
            { estado: "Carabobo", municipio: "Valencia", ciudad: "Valencia", hsp: 5.3, zona: "Centro-Norte" },
            { estado: "Carabobo", municipio: "Puerto Cabello", ciudad: "Puerto Cabello", hsp: 5.5, zona: "Costa" },
            { estado: "Carabobo", municipio: "Naguanagua", ciudad: "Naguanagua", hsp: 5.3, zona: "Centro" },
            { estado: "Lara", municipio: "Iribarren", ciudad: "Barquisimeto", hsp: 5.4, zona: "Centro-Occidente" },
            { estado: "Lara", municipio: "Palavecino", ciudad: "Cabudare", hsp: 5.4, zona: "Centro-Occidente" },
            { estado: "Lara", municipio: "Crespo", ciudad: "Duaca", hsp: 5.2, zona: "Occidente" },
            { estado: "Bolívar", municipio: "Angostura del Orinoco", ciudad: "Ciudad Bolívar", hsp: 5.0, zona: "Sur-Orinoco" },
            { estado: "Bolívar", municipio: "Caroní", ciudad: "Ciudad Guayana (Puerto Ordaz)", hsp: 5.1, zona: "Sur-Orinoco" },
            { estado: "Bolívar", municipio: "Gran Sabana", ciudad: "Santa Elena de Uairén", hsp: 4.8, zona: "Gran Sabana" },
            { estado: "Mérida", municipio: "Libertador", ciudad: "Mérida", hsp: 4.5, zona: "Andes" },
            { estado: "Mérida", municipio: "Campo Elías", ciudad: "Ejido", hsp: 4.5, zona: "Andes" },
            { estado: "Mérida", municipio: "Tovar", ciudad: "Tovar", hsp: 4.6, zona: "Andes" },
            { estado: "Táchira", municipio: "San Cristóbal", ciudad: "San Cristóbal", hsp: 4.4, zona: "Andes" },
            { estado: "Táchira", municipio: "Ayacucho", ciudad: "Colón", hsp: 4.3, zona: "Andes" },
            { estado: "Trujillo", municipio: "Valera", ciudad: "Valera", hsp: 4.7, zona: "Andes" },
            { estado: "Trujillo", municipio: "Trujillo", ciudad: "Trujillo", hsp: 4.6, zona: "Andes" },
            { estado: "Anzoátegui", municipio: "Simón Bolívar", ciudad: "Barcelona", hsp: 5.6, zona: "Nororiente-Costa" },
            { estado: "Anzoátegui", municipio: "Sotillo", ciudad: "Puerto La Cruz", hsp: 5.6, zona: "Nororiente-Costa" },
            { estado: "Anzoátegui", municipio: "Freites", ciudad: "Cantaura", hsp: 5.3, zona: "Llanos" },
            { estado: "Nueva Esparta", municipio: "Mariño", ciudad: "Porlamar", hsp: 6.0, zona: "Insular" },
            { estado: "Nueva Esparta", municipio: "Arismendi", ciudad: "La Asunción", hsp: 6.0, zona: "Insular" },
            { estado: "Sucre", municipio: "Sucre", ciudad: "Cumaná", hsp: 5.8, zona: "Nororiente-Costa" },
            { estado: "Sucre", municipio: "Bermúdez", ciudad: "Carúpano", hsp: 5.7, zona: "Nororiente-Costa" },
            { estado: "Falcón", municipio: "Miranda", ciudad: "Coro", hsp: 5.9, zona: "Costa-Noroeste" },
            { estado: "Falcón", municipio: "Carirubana", ciudad: "Punto Fijo", hsp: 6.0, zona: "Península" },
            { estado: "Barinas", municipio: "Barinas", ciudad: "Barinas", hsp: 5.1, zona: "Llanos" },
            { estado: "Barinas", municipio: "Alberto Arvelo Torrealba", ciudad: "Sabaneta", hsp: 5.1, zona: "Llanos" },
            { estado: "Portuguesa", municipio: "Guanare", ciudad: "Guanare", hsp: 5.2, zona: "Llanos" },
            { estado: "Portuguesa", municipio: "Páez", ciudad: "Acarigua", hsp: 5.3, zona: "Llanos" },
            { estado: "Cojedes", municipio: "San Carlos", ciudad: "San Carlos", hsp: 5.2, zona: "Llanos" },
            { estado: "Guárico", municipio: "Roscio", ciudad: "San Juan de los Morros", hsp: 5.1, zona: "Llanos" },
            { estado: "Guárico", municipio: "Infante", ciudad: "Valle de la Pascua", hsp: 5.2, zona: "Llanos" },
            { estado: "Apure", municipio: "San Fernando", ciudad: "San Fernando de Apure", hsp: 5.0, zona: "Llanos-Sur" },
            { estado: "Apure", municipio: "Páez", ciudad: "Guasdualito", hsp: 4.9, zona: "Llanos-Sur" },
            { estado: "Amazonas", municipio: "Atures", ciudad: "Puerto Ayacucho", hsp: 4.7, zona: "Amazónica" },
            { estado: "Amazonas", municipio: "Alto Orinoco", ciudad: "La Esmeralda", hsp: 4.5, zona: "Amazónica" },
            { estado: "Delta Amacuro", municipio: "Tucupita", ciudad: "Tucupita", hsp: 4.9, zona: "Delta" },
            { estado: "Yaracuy", municipio: "San Felipe", ciudad: "San Felipe", hsp: 5.2, zona: "Centro" },
            { estado: "Aragua", municipio: "Girardot", ciudad: "Maracay", hsp: 5.3, zona: "Centro" },
            { estado: "Aragua", municipio: "Santiago Mariño", ciudad: "Turmero", hsp: 5.3, zona: "Centro" },
            { estado: "Vargas", municipio: "Vargas", ciudad: "La Guaira", hsp: 5.7, zona: "Costa-Central" },
            { estado: "Monagas", municipio: "Maturín", ciudad: "Maturín", hsp: 5.3, zona: "Nororiente" },
            { estado: "Monagas", municipio: "Ezequiel Zamora", ciudad: "Punta de Mata", hsp: 5.2, zona: "Nororiente" }
        ];

        // Elementos DOM
        const selectEstado = document.getElementById('estadoFiltro');
        const selectCiudad = document.getElementById('ciudadFiltro');
        const inputBusqueda = document.getElementById('busqueda');
        // ya no necesitamos tbody, totalSpan, etc., pero los mantenemos para lógica interna
        // pero no se muestran.
        const hspValorSpan = document.getElementById('hspValor');
        const ciudadSeleccionadaSpan = document.getElementById('ciudadSeleccionada');
        const hspActualSpan = document.getElementById('hspActual');

        let currentHSP = 5.0;
        let currentCiudad = "Caracas";

        // Llenar estados
        const estadosUnicos = [...new Set(datosVenezuela.map(d => d.estado))].sort();
        estadosUnicos.forEach(est => {
            const option = document.createElement('option');
            option.value = est;
            option.textContent = est;
            selectEstado.appendChild(option);
        });

        function poblarCiudades() {
            const estadoSel = selectEstado.value;
            let ciudades = [];
            if (estadoSel === 'TODOS') {
                ciudades = [...new Set(datosVenezuela.map(d => d.ciudad))].sort();
            } else {
                ciudades = [...new Set(datosVenezuela.filter(d => d.estado === estadoSel).map(d => d.ciudad))].sort();
            }
            selectCiudad.innerHTML = '<option value="TODAS">Todas las ciudades</option>';
            ciudades.forEach(c => {
                const option = document.createElement('option');
                option.value = c;
                option.textContent = c;
                selectCiudad.appendChild(option);
            });
        }

        // Esta función ya no renderiza tabla, pero la dejamos por si se necesita inicializar algo
        function renderTabla() {
            // No hacemos nada visual, solo para mantener la lógica de selección por si se usara,
            // pero como ocultamos la tabla, el usuario solo usa el selector.
            // Forzamos la actualización de HSP según el selector.
            actualizarPorSelector();
        }

        function actualizarPorSelector() {
            const ciudadSel = selectCiudad.value;
            if (ciudadSel !== 'TODAS') {
                const item = datosVenezuela.find(d => d.ciudad === ciudadSel);
                if (item) {
                    currentHSP = item.hsp;
                    currentCiudad = item.ciudad;
                    hspValorSpan.textContent = item.hsp.toFixed(1);
                    ciudadSeleccionadaSpan.textContent = item.ciudad;
                    hspActualSpan.textContent = item.hsp.toFixed(1);
                }
            } else {
                // Si es TODAS, tomamos la primera de la lista filtrada? O mejor mantener la actual.
                // Por simplicidad, si es TODAS, dejamos el último valor.
                // Pero si no hay ciudad seleccionada, podemos poner Caracas por defecto.
                if (!currentCiudad) {
                    currentHSP = 5.0;
                    currentCiudad = "Caracas";
                    hspValorSpan.textContent = "5.0";
                    ciudadSeleccionadaSpan.textContent = "Caracas";
                    hspActualSpan.textContent = "5.0";
                }
            }
            calcularPaneles();
        }

        selectEstado.addEventListener('change', () => { poblarCiudades(); renderTabla(); actualizarPorSelector(); });
        selectCiudad.addEventListener('change', () => { renderTabla(); actualizarPorSelector(); });
        inputBusqueda.addEventListener('input', renderTabla); // La búsqueda ya no afecta visualmente, pero podría filtrar el selector si quisiéramos. La dejamos por si acaso.

        // Inicializar
        poblarCiudades();
        renderTabla();
        currentHSP = 5.0; currentCiudad = "Caracas";
        hspValorSpan.textContent = "5.0"; ciudadSeleccionadaSpan.textContent = "Caracas"; hspActualSpan.textContent = "5.0";

        // ------------------------------------------------------------
        // FUNCIONES DE CÁLCULO (igual que antes)
        // ------------------------------------------------------------
        function calcularConsumo() {
            let totalDiario = 0, totalInstantaneo = 0;

            // Lámparas LED
            totalDiario += (+document.getElementById('ledWatts').value || 0) * (+document.getElementById('ledCant').value || 0) * (+document.getElementById('ledHoras').value || 0);
            totalInstantaneo += (+document.getElementById('ledWatts').value || 0) * (+document.getElementById('ledCant').value || 0);

            // Reflectores LED
            let reflejoW = +document.getElementById('reflejoTipo').value;
            let reflejoC = +document.getElementById('reflejoCant').value || 0;
            let reflejoH = +document.getElementById('reflejoHoras').value || 0;
            totalDiario += reflejoW * reflejoC * reflejoH;
            totalInstantaneo += reflejoW * reflejoC;

            // Puntos de venta
            let puntoW = +document.getElementById('puntoWatts').value;
            let puntoC = +document.getElementById('puntoCant').value || 0;
            let puntoH = +document.getElementById('puntoHoras').value || 0;
            totalDiario += puntoW * puntoC * puntoH;
            totalInstantaneo += puntoW * puntoC;

            // Computadoras
            let pcW = +document.getElementById('pcTipo').value;
            let pcC = +document.getElementById('pcCant').value || 0;
            let pcH = +document.getElementById('pcHoras').value || 0;
            totalDiario += pcW * pcC * pcH;
            totalInstantaneo += pcW * pcC;

            // Impresoras
            let impW = +document.getElementById('impresoraWatts').value;
            let impC = +document.getElementById('impresoraCant').value || 0;
            let impH = +document.getElementById('impresoraHoras').value || 0;
            totalDiario += impW * impC * impH;
            totalInstantaneo += impW * impC;

            // TV
            totalDiario += (+document.getElementById('tvTipo').value) * (+document.getElementById('tvCant').value || 0) * (+document.getElementById('tvHoras').value || 0);
            totalInstantaneo += (+document.getElementById('tvTipo').value) * (+document.getElementById('tvCant').value || 0);

            // Ventiladores
            totalDiario += (+document.getElementById('ventTipo').value) * (+document.getElementById('ventCant').value || 0) * (+document.getElementById('ventHoras').value || 0);
            totalInstantaneo += (+document.getElementById('ventTipo').value) * (+document.getElementById('ventCant').value || 0);

            // Bomba
            totalDiario += (+document.getElementById('bombaTipo').value) * (+document.getElementById('bombaCant').value || 0) * (+document.getElementById('bombaHoras').value || 0);
            totalInstantaneo += (+document.getElementById('bombaTipo').value) * (+document.getElementById('bombaCant').value || 0);

            // Refrigerador
            let refriVal = document.getElementById('refriTipo').value.split('|');
            let refriDiario = +refriVal[0], refriInst = +refriVal[1];
            let refriC = +document.getElementById('refriCant').value || 0;
            totalDiario += refriDiario * refriC;
            totalInstantaneo += refriInst * refriC;

            // Cargadores
            totalDiario += (+document.getElementById('cargWatts').value || 0) * (+document.getElementById('cargCant').value || 0) * (+document.getElementById('cargHoras').value || 0);
            totalInstantaneo += (+document.getElementById('cargWatts').value || 0) * (+document.getElementById('cargCant').value || 0);

            // Lavadora
            totalDiario += (+document.getElementById('lavaWatts').value || 0) * (+document.getElementById('lavaCant').value || 0) * (+document.getElementById('lavaHoras').value || 0);
            totalInstantaneo += (+document.getElementById('lavaWatts').value || 0) * (+document.getElementById('lavaCant').value || 0);

            // Starlink
            totalDiario += (+document.getElementById('starWatts').value || 0) * (+document.getElementById('starCant').value || 0) * (+document.getElementById('starHoras').value || 0);
            totalInstantaneo += (+document.getElementById('starWatts').value || 0) * (+document.getElementById('starCant').value || 0);

            // Licuadora
            totalDiario += (+document.getElementById('licuWatts').value || 0) * (+document.getElementById('licuCant').value || 0) * (+document.getElementById('licuHoras').value || 0);
            totalInstantaneo += (+document.getElementById('licuWatts').value || 0) * (+document.getElementById('licuCant').value || 0);

            // Aire Acondicionado
            const consumosAire = { "5000": { "24":6000,"12":3000,"6":1500,"4":1000, potencia:500 }, "8000": { "24":9600,"12":4800,"6":2400,"4":1600, potencia:800 }, "12000": { "24":14400,"12":7200,"6":3600,"4":2400, potencia:1200 }, "18000": { "24":21600,"12":10800,"6":5400,"4":3600, potencia:1800 } };
            let aireBTU = document.getElementById('aireBTU').value;
            let aireHoras = document.getElementById('aireHoras').value;
            let aireCant = +document.getElementById('aireCant').value || 0;
            if (consumosAire[aireBTU] && consumosAire[aireBTU][aireHoras] !== undefined) {
                totalDiario += consumosAire[aireBTU][aireHoras] * aireCant;
                totalInstantaneo += consumosAire[aireBTU].potencia * aireCant;
            }

            // Router
            let routerW = +document.getElementById('routerWatts').value;
            let routerC = +document.getElementById('routerCant').value || 0;
            let routerH = +document.getElementById('routerHoras').value || 0;
            totalDiario += routerW * routerC * routerH;
            totalInstantaneo += routerW * routerC;

            // OTROS 1
            totalDiario += (+document.getElementById('otros1Watts').value || 0) * (+document.getElementById('otros1Cant').value || 0) * (+document.getElementById('otros1Horas').value || 0);
            totalInstantaneo += (+document.getElementById('otros1Watts').value || 0) * (+document.getElementById('otros1Cant').value || 0);

            // OTROS 2
            totalDiario += (+document.getElementById('otros2Watts').value || 0) * (+document.getElementById('otros2Cant').value || 0) * (+document.getElementById('otros2Horas').value || 0);
            totalInstantaneo += (+document.getElementById('otros2Watts').value || 0) * (+document.getElementById('otros2Cant').value || 0);

            document.getElementById('resultadoDiario').innerText = Math.round(totalDiario);
            document.getElementById('resultadoInstantaneo').innerText = Math.round(totalInstantaneo);
            calcularPaneles();
        }

        function calcularPaneles() {
            const diario = parseFloat(document.getElementById('resultadoDiario').innerText) || 0;
            const paneles = Math.round(diario / currentHSP);
            document.getElementById('resultadoPaneles').innerText = paneles;
        }

        // Botón Nuevo Cálculo
        document.getElementById('btnNuevoCalculo').addEventListener('click', function() {
            const inputs = document.querySelectorAll('input[type="number"]:not([readonly])');
            inputs.forEach(input => input.value = 0);
            const selects = document.querySelectorAll('select');
            selects.forEach(select => select.selectedIndex = 0);
            calcularConsumo();
        });

        document.querySelector('.btn-calcular').addEventListener('click', calcularConsumo);
        window.onload = calcularConsumo;
    })();
</script>
</body>
</html>
