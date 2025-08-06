estructura del proyecto:
WEB-SUBDIRECCIONG...
├── css
│   └── style.css
├── data
│   └── articles.js
├── js
│   ├── controller.js
│   ├── main.js
│   ├── model.js
│   └── view.js
└── index.html


main.js
// Importamos los datos del modelo
import { articles } from '../data/articles.js';

// El controlador principal de la aplicación
const App = {
    // Inicializa la aplicación
    init() {
        this.renderMenu();
        this.handleRouting();
        window.addEventListener('hashchange', this.handleRouting.bind(this));
        //Tenia un error que no mostraba el contenido al cargar la página y con este evento se soluciona
        //Solución:Debes enlazar el contexto usando .bind(this) en los listeners.
    },

    // Renderiza el menú de navegación
    renderMenu() {
        const menuList = document.getElementById('menu-list');
        menuList.innerHTML = ''; // Limpiar el menú

        articles.forEach(article => {
            const listItem = document.createElement('li');
            const link = document.createElement('a');
            link.href = `#${article.id}`;
            link.textContent = article.title;
            listItem.appendChild(link);
            menuList.appendChild(listItem);
        });
    },

    // Maneja el enrutamiento de la página
    handleRouting() {
        const articleId = window.location.hash.substring(1);
        const article = articles.find(a => a.id === articleId);

        this.renderContent(article);
        this.updateActiveLink(articleId);
    },

    // Renderiza el contenido del artículo en el área principal
    renderContent(article) {
        const contentArea = document.getElementById('content-area');
        if (article) {
            contentArea.innerHTML = `
                <h2>${article.title}</h2>
                <p>${article.description}</p>
            `;
        } else {
            // Contenido por defecto o un mensaje de error si no se encuentra el artículo
            contentArea.innerHTML = `
                <h2>Bienvenido</h2>
                <p>Selecciona un artículo del menú para ver su descripción.</p>
            `;
        }
    },

    // Actualiza la clase 'active' del menú para resaltar el elemento seleccionado
    updateActiveLink(articleId) {
        const links = document.querySelectorAll('#menu-list a');
        links.forEach(link => {
            if (link.href.endsWith(articleId)) {
                link.classList.add('active');
            } else {
                link.classList.remove('active');
            }
        });
    }
};

// Iniciar la aplicación cuando el DOM esté listo
document.addEventListener('DOMContentLoaded', () => App.init());



------ERRORES Y SOLUCIONES----------
Los id: de los articulos no pueden contenter espacios, ya que se toma como un caracter e interrumple la busqueda
por lo tanto no se visualizan en la web. Solucion : Escribir los id: con guiones. Ej: id: "Licencia-por-enfermedad" 
