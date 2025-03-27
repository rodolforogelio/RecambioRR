<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="RecambioRR - Repuestos de coche con envío incluido. Encuentra piezas para todas las marcas.">
    <meta name="keywords" content="repuestos, piezas de coche, mecánica, recambios, envío incluido">
    <meta name="author" content="RecambioRR">
    <title>RecambioRR - Tienda de Piezas de Coche</title>
    <link rel="stylesheet" href="styles.css">
    <script defer src="script.js"></script>
</head>
<body>

    <header>
        <h1>RecambioRR</h1>
        <p>Encuentra cualquier pieza para tu coche con envío incluido.</p>
        <input type="text" id="searchBar" placeholder="Buscar piezas...">
    </header>

    <nav>
        <button class="categoria" onclick="filtrarCategoria('frenos')">Frenos</button>
        <button class="categoria" onclick="filtrarCategoria('motor')">Motor</button>
        <button class="categoria" onclick="filtrarCategoria('eléctrico')">Eléctrico</button>
        <button class="categoria" onclick="filtrarCategoria('todos')">Todos</button>
        <button id="verCarrito">🛒 Carrito (<span id="contadorCarrito">0</span>)</button>
    </nav>

    <main>
        <section id="productos">
            <!-- Los productos se cargarán dinámicamente -->
        </section>
    </main>

    <div id="carritoModal" class="modal">
        <div class="modal-contenido">
            <h2>Carrito de Compras</h2>
            <ul id="listaCarrito"></ul>
            <p id="totalCarrito">Total: €0</p>
            <button onclick="cerrarCarrito()">Cerrar</button>
        </div>
    </div>

    <footer>
        <p>© 2025 RecambioRR - Todos
    los derechos reservados.</p>
    </footer>

</body>
</html>
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    text-align: center;
    background-color: #f4f4f4;
}

header {
    background-color: #0073e6;
    color: white;
    padding: 20px;
}

#searchBar {
    width: 80%;
    padding: 10px;
    margin-top: 10px;
}

nav {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin: 20px;
}

button {
    padding: 10px 15px;
    background-color: #0073e6;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover {
    background-color: #005bb5;
}

#productos {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.producto {
    background: white;
    margin: 15px;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    width: 200px;
    text-align: center;
}

.producto img {
    width: 100%;
    height: auto;
    border-radius: 5px;
}

.modal {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
}

.modal-contenido {
    background: white;
    width: 50%;
    margin: 10% auto;
    padding: 20px;
    border-radius: 5px;
}document.addEventListener("DOMContentLoaded", function() {
    const productos = [
        { nombre: "Filtro de Aceite", imagen: "https://via.placeholder.com/150", precio: 15, categoria: "motor" },
        { nombre: "Pastillas de Freno", imagen: "https://via.placeholder.com/150", precio: 30, categoria: "frenos" },
        { nombre: "Batería 12V", imagen: "https://via.placeholder.com/150", precio: 120, categoria: "eléctrico" }
    ];

    const productosContainer = document.getElementById("productos");
    const searchBar = document.getElementById("searchBar");
    const verCarritoBtn = document.getElementById("verCarrito");
    const carritoModal = document.getElementById("carritoModal");
    const listaCarrito = document.getElementById("listaCarrito");
    const totalCarrito = document.getElementById("totalCarrito");
    const contadorCarrito = document.getElementById("contadorCarrito");

    let carrito = [];

    function mostrarProductos(filtro = "", categoria = "todos") {
        productosContainer.innerHTML = "";
        productos
            .filter(p => 
                p.nombre.toLowerCase().includes(filtro.toLowerCase()) && 
                (categoria === "todos" || p.categoria === categoria)
            )
            .forEach(p => {
                const div = document.createElement("div");
                div.classList.add("producto");
                div.innerHTML = `
                    <img src="${p.imagen}" alt="${p.nombre}">
                    <h3>${p.nombre}</h3>
                    <p>Precio: €${p.precio}</p>
                    <button onclick="agregarAlCarrito('${p.nombre}', ${p.precio})">Añadir al Carrito</button>
                `;
                productosContainer.appendChild(div);
            });
    }

    function filtrarCategoria(categoria) {
        mostrarProductos(searchBar.value, categoria);
    }

    function agregarAlCarrito(nombre, precio) {
        carrito.push({ nombre, precio });
        actualizarCarrito();
    }

    function actualizarCarrito() {
        listaCarrito.innerHTML = "";
        let total = 0;
        carrito.forEach((p, index) => {
            const li = document.createElement("li");
            li.textContent = `${p.nombre} - €${p.precio}`;
            const btnEliminar = document.createElement("button");
            btnEliminar.textContent = "❌";
            btnEliminar.onclick = () => {
                carrito.splice(index, 1);
                actualizarCarrito();
            };
            li.appendChild(btnEliminar);
            listaCarrito.appendChild(li);
            total += p.precio;
        });
        totalCarrito.textContent = `Total: €${total}`;
        contadorCarrito.textContent = carrito.length;
    }

    function abrirCarrito() {
        carritoModal.style.display = "block";
    }

    function cerrarCarrito() {
        carritoModal.style.display = "none";
    }

    verCarritoBtn.addEventListener("click", abrirCarrito);
    searchBar.addEventListener("input", (e) => mostrarProductos(e.target.value));
    mostrarProductos();
});
