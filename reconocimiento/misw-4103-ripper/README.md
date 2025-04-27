# Cambios realizados
- Garantice que no tiene ninguna instalación de Playwrgiht Global
    npm uninstall -g playwright

-Clone el Repositorio en su carpeta de interés
    git clone git@github.com:TheSoftwareDesignLab/RIPuppetCoursera.git .

-Instale las dependencias
    npm install

-Instale Playwright localmente
    npm install playwright

-Instale Globalmente el http-server
    npm install -g http-server

-Detenga cualquier container que esté corriendo Ghost

-Reinicie Ghost
    docker run --name my-ghost -e NODE_ENV=development ghost

-Ejecute la prueba de reconocimiento
    node index.js

-Ejecute el http-server en la carpeta results/
    http-server results

-Acceda a la ruta  http://127.0.0.1:8080 en su navegador (puede cambiar)

-Podrá visualizar el reporte generado.



# Cambios realizados
-Se Renombró el archivo index.html a template.html para no cargar un template vacio y con errores en el momento de ver los reportes.

-Se copia el template.html a la carpeta del reporte como index.html
fs.copyFileSync('./template.html', `${basePath}/index.html`);

-Se copia el main.css a la carpeta del reporte
fs.copyFileSync('./main.css', `${basePath}/main.css`);

-Se copia el index.js a la carpeta del reporte
fs.copyFileSync('./index.js', `${basePath}/index.js`);

-Se automatiza el Setup de Ghost
    if(currentUrl.includes('/setup')) {
        await page.waitForSelector('input[name="blog-title"]');
        await page.fill('input[name="blog-title"]', "Titulo de Blog");
        await page.waitForSelector('input[name="name"]');
        await page.fill('input[name="name"]', "Daniel");  
        await page.waitForSelector('input[name="email"]');
        await page.fill('input[name="email"]', "danielsierra34@gmail.com");  
        await page.waitForSelector('input[name="password"]');
        await page.fill('input[name="password"]', "q1w2e3r4t5y6");  
    }

-Se automatiza el Signup de Ghost
    if(currentUrl.includes('/signin')) {
        await page.waitForSelector('input[name="identification"]');
        await page.fill('input[name="identification"]', "danielsierra34@gmail.com");
        await page.waitForSelector('input[name="password"]');
        await page.fill('input[name="password"]', "q1w2e3r4t5y6");
    }

-Se agrega un TryCatch en el fillInput
    async function fillInput(elementHandle, page) {
        try {
            let type = await page.evaluate(el => {
            return el.type;
            }, elementHandle);

            if (type === 'text') {
            await elementHandle.click();
            await page.keyboard.type(faker.lorem.words());
            } else if (type === 'search') {
            await elementHandle.click();
            await page.keyboard.type(faker.random.alphaNumeric());
            } else if (type === 'password') {
            await elementHandle.click();
            await page.keyboard.type(faker.internet.password());
            } else if (type === 'email') {
            await elementHandle.click();
            await page.keyboard.type(faker.internet.email());
            } else if (type === 'tel') {
            await elementHandle.click();
            await page.keyboard.type(faker.phone.phoneNumber());
            } else if (type === 'number') {
            await elementHandle.click();
            await page.keyboard.type(String(faker.random.numeric(5))); // corregido para usar número
            } else if (type === 'submit' || type === 'radio' || type === 'checkbox') {
            await elementHandle.click();
            }
        } catch (error) {
            if (error.name === 'TimeoutError') {
            console.warn(`Timeout llenando el input. Ignorando...`);
            } else {
            throw error; // Otros errores los seguimos lanzando
            }
        }
    }