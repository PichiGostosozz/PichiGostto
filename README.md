<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GameZone - Tu tienda de videojuegos</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #121212;
            color: #ffffff;
        }
        
        header {
            background-color: #1e1e1e;
            padding: 1rem;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
        }
        
        .logo {
            color: #ff6b6b;
            font-size: 1.8rem;
            font-weight: bold;
            text-align: center;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 1rem;
        }
        
        h1 {
            text-align: center;
            margin: 1.5rem 0;
            color: #ff6b6b;
        }
        
        .games-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
        }
        
        .game-card {
            background-color: #1e1e1e;
            border-radius: 8px;
            overflow: hidden;
            transition: transform 0.3s ease;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        .game-card:hover {
            transform: translateY(-5px);
        }
        
        .game-img {
            width: 100%;
            height: 150px;
            background-color: #333;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
        }
        
        .game-img img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.3s ease;
        }
        
        .game-card:hover .game-img img {
            transform: scale(1.05);
        }
        
        .game-info {
            padding: 1rem;
        }
        
        .game-title {
            font-weight: bold;
            margin-bottom: 0.5rem;
            font-size: 1.1rem;
        }
        
        .game-price {
            color: #4ecca3;
            font-weight: bold;
            margin-bottom: 0.8rem;
        }
        
        .btn {
            display: inline-block;
            padding: 0.5rem 1rem;
            background-color: #ff6b6b;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-weight: bold;
            transition: background-color 0.2s;
            text-decoration: none;
            text-align: center;
        }
        
        .btn:hover {
            background-color: #ff5252;
        }
        
        .btn-full {
            display: block;
            width: 100%;
        }
        
        .modal {
            display: none;
            position: fixed;
            z-index: 100;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            overflow-y: auto;
        }
        
        .modal-content {
            background-color: #232323;
            margin: 2rem auto;
            padding: 1.5rem;
            border-radius: 8px;
            max-width: 500px;
            width: 90%;
            animation: modalFade 0.3s;
        }
        
        @keyframes modalFade {
            from {opacity: 0; transform: translateY(-30px);}
            to {opacity: 1; transform: translateY(0);}
        }
        
        .close {
            color: #aaa;
            float: right;
            font-size: 1.5rem;
            font-weight: bold;
            cursor: pointer;
        }
        
        .close:hover {
            color: white;
        }
        
        .modal-header {
            margin-bottom: 1.5rem;
            padding-bottom: 0.5rem;
            border-bottom: 1px solid #444;
        }
        
        .form-group {
            margin-bottom: 1rem;
        }
        
        label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: bold;
            color: #ccc;
        }
        
        input, select {
            width: 100%;
            padding: 0.8rem;
            border-radius: 4px;
            border: 1px solid #444;
            background-color: #333;
            color: white;
            font-size: 1rem;
        }
        
        .card-details {
            display: grid;
            grid-template-columns: 2fr 1fr 1fr;
            gap: 1rem;
        }
        
        .notification {
            position: fixed;
            bottom: 20px;
            right: 20px;
            padding: 1rem;
            border-radius: 5px;
            color: white;
            font-weight: bold;
            z-index: 1000;
            animation: slideIn 0.3s, fadeOut 0.5s 3s forwards;
            display: none;
        }
        
        .success {
            background-color: #4ecca3;
        }
        
        .error {
            background-color: #ff6b6b;
        }
        
        @keyframes slideIn {
            from {transform: translateX(100%);}
            to {transform: translateX(0);}
        }
        
        @keyframes fadeOut {
            from {opacity: 1;}
            to {opacity: 0;}
        }
        
        @media (max-width: 600px) {
            .card-details {
                grid-template-columns: 1fr;
            }
            
            .modal-content {
                margin: 1rem auto;
                padding: 1rem;
            }
        }
        
        .payment-info {
            margin-top: 1rem;
            padding: 0.8rem;
            background-color: #333;
            border-radius: 4px;
            border-left: 3px solid #4ecca3;
        }
        
        .invoice-number {
            font-family: monospace;
            font-size: 1.1rem;
            color: #4ecca3;
            background-color: #1a1a1a;
            padding: 0.3rem 0.5rem;
            border-radius: 3px;
        }
    </style>
</head>
<body>
    <header>
        <div class="logo">GameZone</div>
    </header>
    
    <div class="container">
        <h1>Catálogo de Videojuegos</h1>
        
        <div class="games-grid" id="gamesGrid">
            <!-- Los juegos se cargan con JavaScript -->
        </div>
    </div>
    
    <!-- Modal de compra -->
    <div id="purchaseModal" class="modal">
        <div class="modal-content">
            <span class="close" id="closeModal">&times;</span>
            <div class="modal-header">
                <h2 id="modalGameTitle">Comprar Juego</h2>
                <p id="modalGamePrice"></p>
            </div>
            
            <form id="paymentForm">
                <div class="form-group">
                    <label for="name">Nombre completo</label>
                    <input type="text" id="name" required>
                </div>
                
                <div class="form-group">
                    <label for="email">Correo electrónico</label>
                    <input type="email" id="email" required>
                </div>
                
                <div class="form-group">
                    <label for="phone">Número de teléfono</label>
                    <input type="tel" id="phone" required>
                </div>
                
                <div class="form-group">
                    <label for="cardNumber">Número de tarjeta</label>
                    <input type="text" id="cardNumber" placeholder="XXXX XXXX XXXX XXXX" maxlength="19" required>
                </div>
                
                <div class="card-details">
                    <div class="form-group">
                        <label for="cardName">Nombre en la tarjeta</label>
                        <input type="text" id="cardName" required>
                    </div>
                    
                    <div class="form-group">
                        <label for="expDate">Fecha exp.</label>
                        <input type="text" id="expDate" placeholder="MM/AA" maxlength="5" required>
                    </div>
                    
                    <div class="form-group">
                        <label for="cvv">CVV</label>
                        <input type="text" id="cvv" maxlength="3" required>
                    </div>
                </div>
                
                <button type="submit" class="btn btn-full">Confirmar Pago</button>
            </form>
        </div>
    </div>
    
    <!-- Modal de confirmación -->
    <div id="confirmationModal" class="modal">
        <div class="modal-content">
            <span class="close" id="closeConfirmation">&times;</span>
            <div class="modal-header">
                <h2>¡Pago Exitoso!</h2>
            </div>
            
            <p>Tu compra ha sido procesada correctamente.</p>
            <div class="payment-info">
                <p><strong>Juego:</strong> <span id="confirmGameTitle"></span></p>
                <p><strong>Precio:</strong> <span id="confirmGamePrice"></span></p>
                <p><strong>Fecha:</strong> <span id="confirmDate"></span></p>
                <p><strong>Número de Factura:</strong> <span id="invoiceNumber" class="invoice-number"></span></p>
            </div>
            <p style="margin-top: 1rem;">Se ha enviado un correo electrónico con los detalles de tu compra.</p>
            
            <button id="closeConfirmButton" class="btn btn-full" style="margin-top: 1.5rem;">Volver a la Tienda</button>
        </div>
    </div>
    
    <div id="notification" class="notification"></div>
    
    <script>
        // Datos de juegos
        const games = [
            { 
                id: 1, 
                title: "Spider-Man 2", 
                price: 29.99, 
                genre: "Aventura",
                image: "https://gmedia.playstation.com/is/image/SIEPDC/spider-man-2-keyart-01-en-7june24?$facebook$"
            },
            { 
                id: 2, 
                title: "Call of Duty: Modern Warfare III", 
                price: 49.99, 
                genre: "Acción",
                image: "https://media.tycsports.com/files/2023/08/09/603287/makarov-call-of-duty-modern-warfare-iii_416x234.webp"
            },
            { 
                id: 3, 
                title: "Final Fantasy XVI", 
                price: 39.99, 
                genre: "RPG",
                image: "https://locosxlosjuegos.com/wp-content/uploads/2023/03/ffxvi.jpg"
            },
            { 
                id: 4, 
                title: "Forza Motorsport", 
                price: 24.99, 
                genre: "Carreras",
                image: "https://cdn.cloudflare.steamstatic.com/steam/apps/2440510/header.jpg"
            },
            { 
                id: 5, 
                title: "Total War: WARHAMMER III", 
                price: 19.99, 
                genre: "Estrategia",
                image: "https://cdn.cloudflare.steamstatic.com/steam/apps/1142710/header.jpg"
            },
            { 
                id: 6, 
                title: "Farming Simulator 22", 
                price: 14.99, 
                genre: "Simulación",
                image: "https://cdn.cloudflare.steamstatic.com/steam/apps/1248130/header.jpg"
            },
            { 
                id: 7, 
                title: "EA Sports FC 24", 
                price: 44.99, 
                genre: "Deportes",
                image: "https://pbs.twimg.com/media/F0TM8n4XgAQCjXm.jpg:large"
            },
            { 
                id: 8, 
                title: "Assassin's Creed Mirage", 
                price: 9.99, 
                genre: "Aventura",
                image: "https://image.api.playstation.com/vulcan/ap/rnd/202310/1616/7dfe05cc56bbe68b29038194fe09cff0b2218cc186496a8f.jpg"
            }
        ];
        
        // Elementos DOM
        const gamesGrid = document.getElementById('gamesGrid');
        const modal = document.getElementById('purchaseModal');
        const closeModal = document.getElementById('closeModal');
        const modalGameTitle = document.getElementById('modalGameTitle');
        const modalGamePrice = document.getElementById('modalGamePrice');
        const paymentForm = document.getElementById('paymentForm');
        const notification = document.getElementById('notification');
        const confirmationModal = document.getElementById('confirmationModal');
        const closeConfirmation = document.getElementById('closeConfirmation');
        const closeConfirmButton = document.getElementById('closeConfirmButton');
        
        // Cargar juegos
        function loadGames() {
            gamesGrid.innerHTML = '';
            
            games.forEach(game => {
                const gameCard = document.createElement('div');
                gameCard.className = 'game-card';
                
                gameCard.innerHTML = `
                    <div class="game-img">
                        <img src="${game.image}" alt="${game.title}" style="width: 100%; height: 100%; object-fit: cover;">
                    </div>
                    <div class="game-info">
                        <div class="game-title">${game.title}</div>
                        <div class="game-genre">${game.genre}</div>
                        <div class="game-price">$${game.price}</div>
                        <button class="btn btn-full buy-btn" data-id="${game.id}">Comprar Ahora</button>
                    </div>
                `;
                
                gamesGrid.appendChild(gameCard);
            });
            
            // Agregar eventos a los botones de compra
            document.querySelectorAll('.buy-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const gameId = parseInt(this.getAttribute('data-id'));
                    openPurchaseModal(gameId);
                });
            });
        }
        
        // Abrir modal de compra
        function openPurchaseModal(gameId) {
            const game = games.find(g => g.id === gameId);
            
            if (game) {
                modalGameTitle.textContent = `Comprar ${game.title}`;
                modalGamePrice.textContent = `Precio: $${game.price}`;
                modal.style.display = 'block';
                
                // Guardar ID del juego en el formulario
                paymentForm.setAttribute('data-game-id', gameId);
            }
        }
        
        // Cerrar modal
        closeModal.addEventListener('click', function() {
            modal.style.display = 'none';
        });
        
        window.addEventListener('click', function(event) {
            if (event.target === modal) {
                modal.style.display = 'none';
            }
            if (event.target === confirmationModal) {
                confirmationModal.style.display = 'none';
            }
        });
        
        // Formatear número de tarjeta
        const cardNumber = document.getElementById('cardNumber');
        cardNumber.addEventListener('input', function(e) {
            let value = e.target.value.replace(/\s+/g, '').replace(/[^0-9]/gi, '');
            let formattedValue = '';
            
            for (let i = 0; i < value.length; i++) {
                if (i > 0 && i % 4 === 0) {
                    formattedValue += ' ';
                }
                formattedValue += value[i];
            }
            
            e.target.value = formattedValue;
        });
        
        // Formatear fecha de expiración
        const expDate = document.getElementById('expDate');
        expDate.addEventListener('input', function(e) {
            let value = e.target.value.replace(/[^0-9]/g, '');
            if (value.length > 2) {
                value = value.substring(0, 2) + '/' + value.substring(2, 4);
            }
            e.target.value = value;
        });
        
        // Procesar pago con tarjeta
        paymentForm.addEventListener('submit', function(e) {
            e.preventDefault();
            processCardPayment();
        });
        
        // Eventos para el modal de confirmación
        closeConfirmation.addEventListener('click', function() {
            confirmationModal.style.display = 'none';
        });
        
        closeConfirmButton.addEventListener('click', function() {
            confirmationModal.style.display = 'none';
        });
        
        // Procesar pago con tarjeta
        function processCardPayment() {
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            const phone = document.getElementById('phone').value;
            const cardNum = document.getElementById('cardNumber').value;
            const cardName = document.getElementById('cardName').value;
            const expiry = document.getElementById('expDate').value;
            const cvv = document.getElementById('cvv').value;
            
            // Validaciones básicas
            if (!name || !email || !phone || !cardNum || !cardName || !expiry || !cvv) {
                showNotification('Por favor, completa todos los campos', 'error');
                return;
            }
            
            if (cardNum.replace(/\s/g, '').length !== 16) {
                showNotification('Número de tarjeta inválido', 'error');
                return;
            }
            
            if (!/^\d{2}\/\d{2}$/.test(expiry)) {
                showNotification('Formato de fecha inválido (MM/AA)', 'error');
                return;
            }
            
            if (cvv.length !== 3) {
                showNotification('CVV inválido', 'error');
                return;
            }
            
            // Simulación de pago
            simulatePayment(email);
        }
        
        // Simular pago (éxito en la mayoría de casos)
        function simulatePayment(email) {
            const gameId = parseInt(paymentForm.getAttribute('data-game-id'));
            const game = games.find(g => g.id === gameId);
            
            // 90% de probabilidad de éxito
            const isSuccess = Math.random() < 0.9;
            
            setTimeout(() => {
                if (isSuccess) {
                    // Generar número de factura único
                    const invoiceNumber = generateInvoiceNumber();
                    
                    // Mostrar notificación de éxito
                    showNotification('¡Pago realizado exitosamente!', 'success');
                    
                    // Cerrar modal de pago
                    modal.style.display = 'none';
                    
                    // Preparar modal de confirmación
                    document.getElementById('confirmGameTitle').textContent = game.title;
                    document.getElementById('confirmGamePrice').textContent = `$${game.price}`;
                    document.getElementById('confirmDate').textContent = new Date().toLocaleString();
                    document.getElementById('invoiceNumber').textContent = invoiceNumber;
                    
                    // Mostrar modal de confirmación después de un pequeño retraso
                    setTimeout(() => {
                        confirmationModal.style.display = 'block';
                    }, 500);
                    
                    // Simulación de envío de correo
                    console.log(`
                        CORREO ENVIADO A: ${email}
                        ASUNTO: Confirmación de compra - GameZone
                        CONTENIDO: 
                        ¡Gracias por tu compra en GameZone!
                        
                        Detalles de la transacción:
                        Juego: ${game.title}
                        Precio: $${game.price}
                        Fecha: ${new Date().toLocaleString()}
                        Número de Factura: ${invoiceNumber}
                        
                        Tu clave de acceso para descargar el juego es: ${generateGameKey()}
                    `);
                    
                    // Simulación de notificación al vendedor (tú)
                    console.log(`
                        NOTIFICACIÓN AL VENDEDOR:
                        Nueva venta realizada:
                        Juego: ${game.title}
                        Precio: $${game.price}
                        Cliente: ${document.getElementById('name').value}
                        Email: ${email}
                        Teléfono: ${document.getElementById('phone').value}
                        Número de Factura: ${invoiceNumber}
                        
                        El pago de $${game.price} ha sido depositado en tu cuenta de PayPal.
                    `);
                    
                    // Reiniciar formulario
                    paymentForm.reset();
                    
                } else {
                    showNotification('Pago rechazado. Inténtalo de nuevo.', 'error');
                }
            }, 1500);
        }
        
        // Generar número de factura
        function generateInvoiceNumber() {
            const prefix = "GZ";
            const timestamp = Date.now().toString().slice(-8);
            const random = Math.floor(Math.random() * 1000).toString().padStart(3, '0');
            return `${prefix}-${timestamp}-${random}`;
        }
        
        // Generar clave de juego
        function generateGameKey() {
            const chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
            let key = "";
            
            // Formato: XXXX-XXXX-XXXX-XXXX
            for (let j = 0; j < 4; j++) {
                for (let i = 0; i < 4; i++) {
                    key += chars.charAt(Math.floor(Math.random() * chars.length));
                }
                if (j < 3) key += "-";
            }
            
            return key;
        }
        
        // Mostrar notificación
        function showNotification(message, type) {
            notification.textContent = message;
            notification.className = `notification ${type}`;
            notification.style.display = 'block';
            
            // Ocultar notificación después de 4 segundos
            setTimeout(() => {
                notification.style.display = 'none';
            }, 4000);
        }
        
        // Inicializar la página
        loadGames();
    </script>
</body>
</html>
