# ASISTENCIA

<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Verificación de Ubicación - Asistencia</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .container {
            max-width: 500px;
            width: 100%;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
            overflow: hidden;
            animation: slideIn 0.6s ease-out;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(50px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .header {
            background: linear-gradient(135deg, #2c3e50, #34495e);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 {
            font-size: 1.8em;
            margin-bottom: 10px;
        }

        .header p {
            font-size: 0.95em;
            opacity: 0.9;
        }

        .content {
            padding: 40px;
            text-align: center;
        }

        .location-status {
            background: #fff3e0;
            border: 3px solid #ffb74d;
            border-radius: 15px;
            padding: 25px;
            margin: 25px 0;
            font-size: 16px;
            font-weight: 500;
            color: #f57c00;
            transition: all 0.4s ease;
        }

        .location-status.checking {
            background: #e3f2fd;
            border-color: #42a5f5;
            color: #1976d2;
        }

        .location-status.success {
            background: #e8f5e8;
            border-color: #4caf50;
            color: #2e7d32;
        }

        .location-status.error {
            background: #ffebee;
            border-color: #f44336;
            color: #c62828;
        }

        .location-status.denied {
            background: #fce4ec;
            border-color: #e91e63;
            color: #ad1457;
        }

        .loading {
            display: inline-block;
            width: 24px;
            height: 24px;
            border: 3px solid #f3f3f3;
            border-top: 3px solid #1976d2;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin-right: 10px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .btn {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            padding: 15px 30px;
            border: none;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: inline-flex;
            align-items: center;
            gap: 10px;
            text-decoration: none;
            margin: 15px 10px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
        }

        .btn:disabled {
            background: #6c757d;
            cursor: not-allowed;
            transform: none;
        }

        .btn-success {
            background: linear-gradient(135deg, #28a745, #20c997);
            font-size: 18px;
            padding: 18px 35px;
            box-shadow: 0 8px 25px rgba(40, 167, 69, 0.3);
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { box-shadow: 0 8px 25px rgba(40, 167, 69, 0.3); }
            50% { box-shadow: 0 8px 35px rgba(40, 167, 69, 0.5); }
            100% { box-shadow: 0 8px 25px rgba(40, 167, 69, 0.3); }
        }

        .location-details {
            background: #f8f9fa;
            border-radius: 12px;
            padding: 20px;
            margin: 20px 0;
            text-align: left;
            font-size: 14px;
            color: #495057;
            display: none;
        }

        .location-details.show {
            display: block;
            animation: fadeIn 0.5s ease-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .detail-row {
            display: flex;
            justify-content: space-between;
            margin: 8px 0;
            padding: 8px 0;
            border-bottom: 1px solid #dee2e6;
        }

        .detail-row:last-child {
            border-bottom: none;
        }

        .instructions {
            background: #e8f4fd;
            border: 2px solid #bee5eb;
            border-radius: 12px;
            padding: 20px;
            margin: 20px 0;
            text-align: left;
            font-size: 14px;
            color: #0c5460;
        }

        .instructions h4 {
            margin-bottom: 12px;
            color: #0c5460;
        }

        .instructions ol {
            margin-left: 20px;
        }

        .instructions li {
            margin: 8px 0;
            line-height: 1.5;
        }

        .error-message {
            background: #f8d7da;
            border: 2px solid #f5c6cb;
            border-radius: 12px;
            padding: 20px;
            margin: 20px 0;
            color: #721c24;
            text-align: left;
        }

        @media (max-width: 600px) {
            .container {
                margin: 10px;
                border-radius: 15px;
            }
            
            .header {
                padding: 25px 20px;
            }
            
            .content {
                padding: 30px 25px;
            }
            
            .btn {
                width: 100%;
                justify-content: center;
                margin: 10px 0;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>📍 Verificación de Ubicación</h1>
            <p>Sistema de Control de Asistencia</p>
        </div>

        <div class="content">
            <div class="instructions">
                <h4>📋 Instrucciones:</h4>
                <ol>
                    <li>Presiona el botón "Verificar mi ubicación"</li>
                </ol>
            </div>

            <div class="location-status" id="locationStatus">
                📍 Presiona el botón para verificar tu ubicación
            </div>

            <div class="location-details" id="locationDetails">
                <strong>📊 Detalles de ubicación:</strong>
                <div class="detail-row">
                    <span>Tu latitud:</span>
                    <span id="userLat">-</span>
                </div>
                <div class="detail-row">
                    <span>Tu longitud:</span>
                    <span id="userLon">-</span>
                </div>
                <div class="detail-row">
                    <span>Precisión GPS:</span>
                    <span id="accuracy">-</span>
                </div>
                <div class="detail-row">
                    <span>Distancia al centro:</span>
                    <span id="distance">-</span>
                </div>
                <div class="detail-row">
                    <span>Estado:</span>
                    <span id="status">-</span>
                </div>
                <div class="detail-row">
                    <span>Verificado:</span>
                    <span id="timestamp">-</span>
                </div>
            </div>

            <button class="btn" id="checkLocationBtn" onclick="checkLocation()">
                📍 Verificar mi ubicación
            </button>

            <div id="formAccess" style="display: none;">
                <a href="https://forms.cloud.microsoft/r/0TfgMxTLD4?origin=lprLink" 
                   target="_blank" 
                   class="btn btn-success">
                    📝 Acceder al Formulario de Asistencia
                </a>
                <p style="color: #28a745; font-weight: 600; margin-top: 15px;">
                    ✅ Ubicación verificada correctamente. Puedes proceder con el registro de asistencia.
                </p>
            </div>

            <div id="errorSection" style="display: none;">
                <div class="error-message">
                    <h4>❌ Acceso denegado</h4>
                    <p id="errorMessage"></p>
                    <p style="margin-top: 15px;">
                        <strong>Posibles soluciones:</strong>
                    </p>
                    <ul style="margin-left: 20px; margin-top: 10px;">
                        <li>Asegúrate de estar físicamente presente en el área de clase</li>
                        <li>Verifica que tu GPS esté activado</li>
                        <li>Intenta salir al exterior para mejorar la señal GPS</li>
                        <li>Contacta al profesor si el problema persiste</li>
                    </ul>
                </div>
                <button class="btn" onclick="retryLocation()">
                    🔄 Intentar nuevamente
                </button>
            </div>
        </div>
    </div>

    <script>
        // Configuración del sistema
        const TARGET_LOCATION = {
            latitude: -17.8121394,
            longitude: -63.178628,
            tolerance: 200, // Radio en metros
            name: "Área de Clase Autorizada"
        };

        const FORMS_URL = "https://forms.cloud.microsoft/r/0TfgMxTLD4?origin=lprLink";

        let currentUserLocation = null;

        // Función principal para verificar ubicación
        function checkLocation() {
            const statusDiv = document.getElementById('locationStatus');
            const checkBtn = document.getElementById('checkLocationBtn');
            const detailsDiv = document.getElementById('locationDetails');
            const formAccess = document.getElementById('formAccess');
            const errorSection = document.getElementById('errorSection');

            // Resetear estados
            formAccess.style.display = 'none';
            errorSection.style.display = 'none';
            detailsDiv.classList.remove('show');

            // Verificar soporte de geolocalización
            if (!navigator.geolocation) {
                showError('Tu dispositivo no soporta geolocalización. Usa un navegador moderno.');
                return;
            }

            // Cambiar estado a verificando
            statusDiv.className = 'location-status checking';
            statusDiv.innerHTML = '<div class="loading"></div>Obteniendo tu ubicación GPS...';
            checkBtn.disabled = true;
            checkBtn.innerHTML = '<div class="loading"></div>Verificando...';

            // Opciones de geolocalización de alta precisión
            const options = {
                enableHighAccuracy: true,
                timeout: 20000, // 20 segundos
                maximumAge: 60000 // Cache de 1 minuto
            };

            // Obtener ubicación
            navigator.geolocation.getCurrentPosition(
                (position) => {
                    currentUserLocation = {
                        latitude: position.coords.latitude,
                        longitude: position.coords.longitude,
                        accuracy: position.coords.accuracy,
                        timestamp: new Date()
                    };

                    processLocation(currentUserLocation);
                },
                (error) => {
                    handleLocationError(error);
                },
                options
            );
        }

        // Procesar la ubicación obtenida
        function processLocation(location) {
            const statusDiv = document.getElementById('locationStatus');
            const checkBtn = document.getElementById('checkLocationBtn');
            const detailsDiv = document.getElementById('locationDetails');

            // Calcular distancia
            const distance = calculateDistance(
                location.latitude,
                location.longitude,
                TARGET_LOCATION.latitude,
                TARGET_LOCATION.longitude
            );

            // Actualizar detalles
            updateLocationDetails(location, distance);
            detailsDiv.classList.add('show');

            // Verificar si está dentro del rango permitido
            const isWithinRange = distance <= TARGET_LOCATION.tolerance;
            
            checkBtn.disabled = false;
            checkBtn.innerHTML = '📍 Verificar nuevamente';

            if (isWithinRange) {
                // ACCESO AUTORIZADO
                statusDiv.className = 'location-status success';
                statusDiv.innerHTML = `
                    ✅ <strong>Ubicación verificada correctamente</strong><br>
                    Estás a ${Math.round(distance)}m del área autorizada<br>
                    <small>Dentro del rango permitido de ${TARGET_LOCATION.tolerance}m</small>
                `;
                
                // Mostrar acceso al formulario
                document.getElementById('formAccess').style.display = 'block';
                document.getElementById('errorSection').style.display = 'none';
                
            } else {
                // ACCESO DENEGADO
                statusDiv.className = 'location-status error';
                statusDiv.innerHTML = `
                    ❌ <strong>Ubicación fuera del área autorizada</strong><br>
                    Estás a ${Math.round(distance)}m del área de clase<br>
                    <small>Distancia máxima permitida: ${TARGET_LOCATION.tolerance}m</small>
                `;
                
                // Mostrar sección de error
                const errorMsg = `Estás a ${Math.round(distance)} metros del área de clase. Necesitas estar dentro de un radio de ${TARGET_LOCATION.tolerance} metros para acceder al formulario.`;
                showError(errorMsg);
            }
        }

        // Manejar errores de geolocalización
        function handleLocationError(error) {
            const statusDiv = document.getElementById('locationStatus');
            const checkBtn = document.getElementById('checkLocationBtn');
            
            checkBtn.disabled = false;
            checkBtn.innerHTML = '📍 Verificar mi ubicación';

            let errorMessage = '';
            
            switch(error.code) {
                case error.PERMISSION_DENIED:
                    statusDiv.className = 'location-status denied';
                    statusDiv.innerHTML = '🚫 <strong>Acceso a ubicación denegado</strong><br>Debes permitir el acceso para continuar';
                    errorMessage = 'Has denegado el acceso a tu ubicación. Debes permitir que el navegador acceda a tu GPS para poder registrar tu asistencia.';
                    break;
                    
                case error.POSITION_UNAVAILABLE:
                    statusDiv.className = 'location-status error';
                    statusDiv.innerHTML = '📍 <strong>Ubicación no disponible</strong><br>No se pudo determinar tu posición';
                    errorMessage = 'No se pudo obtener tu ubicación. Verifica que tu GPS esté activado y que tengas señal.';
                    break;
                    
                case error.TIMEOUT:
                    statusDiv.className = 'location-status error';
                    statusDiv.innerHTML = '⏱️ <strong>Tiempo agotado</strong><br>La búsqueda de ubicación tardó demasiado';
                    errorMessage = 'La búsqueda de ubicación se agotó. Verifica tu conexión y señal GPS, luego intenta nuevamente.';
                    break;
                    
                default:
                    statusDiv.className = 'location-status error';
                    statusDiv.innerHTML = '❌ <strong>Error desconocido</strong><br>No se pudo obtener tu ubicación';
                    errorMessage = 'Ocurrió un error inesperado al obtener tu ubicación. Intenta nuevamente o contacta al soporte técnico.';
                    break;
            }
            
            showError(errorMessage);
        }

        // Actualizar detalles de ubicación
        function updateLocationDetails(location, distance) {
            document.getElementById('userLat').textContent = location.latitude.toFixed(6);
            document.getElementById('userLon').textContent = location.longitude.toFixed(6);
            document.getElementById('accuracy').textContent = `±${Math.round(location.accuracy)}m`;
            document.getElementById('distance').textContent = `${Math.round(distance)}m`;
            document.getElementById('status').textContent = distance <= TARGET_LOCATION.tolerance ? 'Autorizado' : 'Fuera de rango';
            document.getElementById('timestamp').textContent = location.timestamp.toLocaleTimeString();
        }

        // Mostrar mensaje de error
        function showError(message) {
            document.getElementById('errorMessage').innerHTML = message;
            document.getElementById('errorSection').style.display = 'block';
            document.getElementById('formAccess').style.display = 'none';
        }

        // Reintentar verificación
        function retryLocation() {
            document.getElementById('errorSection').style.display = 'none';
            checkLocation();
        }

        // Calcular distancia entre dos puntos GPS (fórmula de Haversine)
        function calculateDistance(lat1, lon1, lat2, lon2) {
            const R = 6371000; // Radio de la Tierra en metros
            const dLat = (lat2 - lat1) * Math.PI / 180;
            const dLon = (lon2 - lon1) * Math.PI / 180;
            
            const a = Math.sin(dLat/2) * Math.sin(dLat/2) +
                      Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) *
                      Math.sin(dLon/2) * Math.sin(dLon/2);
            
            const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
            return R * c; // Distancia en metros
        }

        // Inicializar cuando se carga la página
        document.addEventListener('DOMContentLoaded', function() {
            console.log('Sistema de verificación de ubicación iniciado');
            console.log(`Ubicación objetivo: ${TARGET_LOCATION.latitude}, ${TARGET_LOCATION.longitude}`);
            console.log(`Radio permitido: ${TARGET_LOCATION.tolerance}m`);
        });
    </script>
</body>
</html>
