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


a modificar:
articles.js
export const articles = [
    {
        id: 'ley-6137-articulo-1',
        title: 'LEY Nº 6137 - Artículo 1º',
        description: 'MODIFÍCASE el artículo 141° de la Ley 4.067, que quedará redactado de la siguiente manera: "Artículo 141°. El personal femenino podrá hacer uso de ciento ochenta (180) días corridos de licencia por maternidad, con goce íntegro de haberes. Esta licencia comenzará a contarse a partir de los siete meses de embarazo, el que se acreditará mediante la presentación del certificado médico."'
    },
    {
        id: 'ley-6137-articulo-3',
        title: 'LEY Nº 6137 - Artículo 3º',
        description: 'MODIFÍCASE el artículo 144º de la ley 4.067, que quedará redactado de la siguiente manera: "Artículo 144. El término de ciento ochenta (180) días de licencia podrá modificarse en los siguientes casos: a) nacimientos múltiples: cuando se produzca el nacimiento de dos (2) o más niños se otorgará ciento noventa (190) días corridos de licencia, la que se ampliará a doscientos (200) días corridos cuando sean prematuros; b) si se produjera defunción fetal, entre el séptimo y noveno mes de gestación, se otorgará sesenta (60) días corridos de licencia, sin perjuicio de otros que podrán acordarse por enfermedad; c) si la defunción fetal, se produjera entre el cuarto y sexto mes de gestación se otorgarán treinta (30) días corridos de licencia, sin perjuicio de otros que podrían concederse por enfermedad; d) si se produjera la interrupción del embarazo, antes de los tres meses, se podrá conceder hasta quince (15) días de licencia".'
    },
    {
        id: 'anexo-ii-capitulo-ii-articulo-8',
        title: 'ANEXO II - Artículo 8º',
        description: 'Las licencias especiales para el tratamiento de la salud serán otorgadas con goce íntegro o parcial de haberes según se especifique en cada caso. a) Para el tratamiento de afecciones comunes, que inhabiliten para el desempeño del trabajo, ... se concederán al agente hasta treinta (30) días corridos, continuos o discontinuos, con percepción integra de haberes, durante el año calendario. Vencido este plazo, cualquier otra licencia que se conceda durante el curso del año por las mismas causas, será sin goce de haberes. b) Para el tratamiento de afecciones que por su naturaleza y evolución requieran prolongada asistencia médica... se concederán hasta dos (2) años de licencia con goce íntegro de haberes, y un (1) año más con el 50% de los mismos. Esta licencia se otorgará por días corridos, continuos o discontinuos.'
    },
    {
        id: 'anexo-ii-capitulo-ii-articulo-13',
        title: 'ANEXO II - Artículo 13º',
        description: 'a) El personal docente femenino gozará de noventa (90) días corridos de licencia por maternidad, con goce íntegro de haberes, fraccionados en dos (2) períodos de cuarenta y cinco (45) días, uno anterior y otro posterior al parto. En ningún caso la licencia anterior al parto podrá ser inferior a treinta (30) días; en este supuesto, el resto del período total se acumulará al período de descanso posterior al parto.'
    },
];


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