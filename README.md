<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Ally.Boutyque</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #fff;
      color: #333;
    }

    header {
      background-color: #ffb6c1;
      padding: 20px;
      text-align: center;
    }

    header h1 {
      margin: 0;
      color: white;
      font-size: 2.5rem;
    }

    nav {
      background-color: #ffe4e1;
      padding: 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    nav .cart-button,
    nav .login-button,
    nav .register-button {
      background-color: #ff69b4;
      border: none;
      color: white;
      padding: 10px 20px;
      border-radius: 5px;
      cursor: pointer;
      margin-left: 10px;
    }

    .container {
      padding: 30px;
      max-width: 1200px;
      margin: auto;
    }

    .upload-section {
      margin-bottom: 20px;
    }

    .upload-section input[type="file"] {
      margin-right: 10px;
    }

    .products {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
    }

    .product {
      background-color: #ffe4e1;
      border: 1px solid #ffc0cb;
      border-radius: 10px;
      padding: 15px;
      text-align: center;
      transition: transform 0.2s;
      position: relative;
    }

    .product:hover {
      transform: scale(1.05);
    }

    .product img {
      width: 100%;
      height: 200px;
      object-fit: cover;
      border-radius: 8px;
      background-color: #fff0f5;
    }

    .product h2 {
      margin-top: 10px;
      font-size: 1.2rem;
    }

    .product p {
      color: #555;
    }

    .product button {
      background-color: #ff69b4;
      border: none;
      color: white;
      padding: 10px 15px;
      border-radius: 5px;
      margin-top: 10px;
      cursor: pointer;
    }

    .remove-button {
      background-color: #ff4d6d;
      margin-left: 10px;
    }

    footer {
      background-color: #ffb6c1;
      color: white;
      text-align: center;
      padding: 15px 0;
      margin-top: 40px;
    }

    footer a {
      color: white;
      text-decoration: underline;
    }

    #login-modal,
    #register-modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5);
      justify-content: center;
      align-items: center;
    }

    .modal-content {
      background: white;
      padding: 20px;
      border-radius: 10px;
      width: 300px;
      text-align: center;
    }

    .modal-content input {
      width: 100%;
      padding: 8px;
      margin: 10px 0;
    }
  </style>
</head>
<body>
  <header>
    <h1>Ally.Boutyque</h1>
  </header>

  <nav>
    <span>Bem-vinda à sua loja favorita de bolsas!</span>
    <div>
      <button class="register-button" onclick="openRegister()">📝 Cadastrar</button>
      <button class="login-button" onclick="openLogin()">🔐 Login</button>
      <button class="cart-button" onclick="viewCart()">👜 Carrinho (<span id="cart-count">0</span>)</button>
    </div>
  </nav>

  <div class="container">
    <div class="upload-section" id="upload-section">
      <input type="file" accept="image/*" id="imageUpload" />
      <button onclick="uploadImage()">Adicionar Bolsa</button>
    </div>
    <div class="products" id="product-list">
      <!-- Produtos inseridos via JavaScript -->
    </div>
  </div>

  <footer>
    <p>&copy; 2025 Ally.Boutyque. Todos os direitos reservados.</p>
    <p>Siga-nos no Instagram: <a href="https://instagram.com/ally.boutyque" target="_blank">@ally.boutyque</a></p>
  </footer>

  <div id="login-modal">
    <div class="modal-content">
      <h2>Login</h2>
      <input type="text" id="username" placeholder="Usuário" />
      <input type="password" id="password" placeholder="Senha" />
      <button onclick="loginUser()">Entrar</button>
      <br /><br />
      <button onclick="closeLogin()">Cancelar</button>
    </div>
  </div>

  <div id="register-modal">
    <div class="modal-content">
      <h2>Cadastro</h2>
      <input type="text" id="new-username" placeholder="Novo usuário" />
      <input type="password" id="new-password" placeholder="Nova senha" />
      <button onclick="registerUser()">Cadastrar</button>
      <br /><br />
      <button onclick="closeRegister()">Cancelar</button>
    </div>
  </div>

  <script>
    let cart = [];
    let loggedIn = false;
    const users = { cliente: '1234', admin: 'admin123' };
    let currentUser = null;

    function addToCart(product) {
      if (!loggedIn) {
        alert('Por favor, faça login para adicionar ao carrinho.');
        return;
      }
      cart.push(product);
      document.getElementById('cart-count').textContent = cart.length;
      alert(product + ' adicionada ao carrinho!');
    }

    function viewCart() {
      if (!loggedIn) {
        alert('Por favor, faça login para visualizar o carrinho.');
        return;
      }
      if (cart.length === 0) {
        alert('Seu carrinho está vazio.');
      } else {
        alert('Itens no carrinho:\n' + cart.join('\n'));
      }
    }

    function openLogin() {
      document.getElementById('login-modal').style.display = 'flex';
    }

    function closeLogin() {
      document.getElementById('login-modal').style.display = 'none';
    }

    function openRegister() {
      document.getElementById('register-modal').style.display = 'flex';
    }

    function closeRegister() {
      document.getElementById('register-modal').style.display = 'none';
    }

    function loginUser() {
      const username = document.getElementById('username').value;
      const password = document.getElementById('password').value;

      if (users[username] && users[username] === password) {
        loggedIn = true;
        currentUser = username;
        closeLogin();
        alert('Login bem-sucedido!');
        if (username !== 'admin') {
          document.getElementById('upload-section').style.display = 'none';
        } else {
          document.getElementById('upload-section').style.display = 'block';
        }
      } else {
        alert('Usuário ou senha incorretos.');
      }
    }

    function registerUser() {
      const newUsername = document.getElementById('new-username').value;
      const newPassword = document.getElementById('new-password').value;

      if (users[newUsername]) {
        alert('Usuário já existe!');
      } else {
        users[newUsername] = newPassword;
        closeRegister();
        alert('Cadastro realizado com sucesso! Faça login.');
      }
    }

    function loadProducts() {
      const productList = document.getElementById('product-list');
      for (let i = 1; i <= 15; i++) {
        const price = (149.9 + (i - 1) * 10).toFixed(2);
        const product = document.createElement('div');
        product.className = 'product';
        product.innerHTML = `
          <img src="https://via.placeholder.com/300x200.png?text=Bolsa+${i}" alt="Bolsa ${i}" />
          <h2>Bolsa Modelo ${i}</h2>
          <p>R$ ${price}</p>
          <button onclick="${loggedIn ? `addToCart('Bolsa Modelo ${i}')` : `alert('Faça login para adicionar ao carrinho.')`}">Adicionar ao Carrinho</button>
        `;
        productList.appendChild(product);
      }
    }

    function uploadImage() {
      if (currentUser !== 'admin') {
        alert('Apenas o administrador pode adicionar bolsas.');
        return;
      }

      const input = document.getElementById('imageUpload');
      const file = input.files[0];

      if (file && file.type.startsWith('image/')) {
        const reader = new FileReader();
        reader.onload = function (e) {
          const productList = document.getElementById('product-list');
          const product = document.createElement('div');
          product.className = 'product';
          product.innerHTML = `
            <img src="${e.target.result}" alt="Nova Bolsa" />
            <h2>Nova Bolsa</h2>
            <p>R$ 199,90</p>
            <button onclick="${loggedIn ? `addToCart('Nova Bolsa')` : `alert('Faça login para adicionar ao carrinho.')`}">Adicionar ao Carrinho</button>
            <button class="remove-button" onclick="removeProduct(this)">Remover</button>
          `;
          productList.appendChild(product);
          input.value = ''; // limpa o input
        };
        reader.readAsDataURL(file);
      } else {
        alert('Por favor, selecione uma imagem válida.');
      }
    }

    function removeProduct(button) {
      if (currentUser !== 'admin') {
        alert('Apenas o administrador pode remover bolsas.');
        return;
      }
      const productDiv = button.parentElement;
      productDiv.remove();
    }

    window.onload = loadProducts;
  </script>
</body>
</html>
