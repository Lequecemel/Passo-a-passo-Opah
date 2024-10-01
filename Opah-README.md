
# Projeto de Automação de Testes Mobile

Este projeto é uma automação de testes para um aplicativo mobile utilizando WebdriverIO, Appium e integração com BrowserStack.

## Funcionalidades Cobertas

- **Login/Cadastro**
- **Navegação entre telas**
- **Preenchimento de formulários**
- **Verificação de mensagens de erro**

## Como Executar os Testes

### Requisitos

- Node.js v12+
- Appium Server
- Dispositivos/emuladores configurados

### Instalação

1. Clone o repositório:
   ```bash
   git clone https://github.com/yourusername/mobile-test-project.git
   ```

2. Instale as dependências:
   ```bash
   npm install
   ```

3. Configure o WebdriverIO com suas credenciais do BrowserStack (se necessário).

### Executar os Testes

Para rodar os testes, use o comando:
```bash
npm run test
```

Os testes serão executados em dispositivos Android e iOS simulados.

### Relatórios de Testes

Os relatórios são gerados automaticamente com o Allure Report. Para visualizá-los:
```bash
allure serve allure-results
```

## Estrutura do Projeto

```bash
/mobile-test-project
|-- /test
    |-- /pageobjects
        |-- login.page.js
        |-- form.page.js
    |-- /data
        |-- loginData.json
    |-- login.spec.js
|-- /screenshots
|-- .gitignore
|-- README.md
|-- wdio.conf.js
|-- package.json
|-- .gitlab-ci.yml
```

- `/test`: Contém os scripts de teste.
- `/pageobjects`: Implementação do padrão Page Object.
- `/data`: Arquivos de dados para testes data-driven.
- `/screenshots`: Capturas de tela geradas após falhas nos testes.

## CI/CD

Este projeto está integrado ao GitLab CI/CD para execução automática dos testes após cada commit.

### 3. wdio.conf.js
O arquivo de configuração do WebdriverIO:

```javascript
exports.config = {
    runner: 'local',
    port: 4723,
    specs: ['./test/**/*.spec.js'],
    maxInstances: 1,
    capabilities: [{
        platformName: 'Android',
        'appium:platformVersion': '11.0',
        'appium:deviceName': 'Pixel_3',
        'appium:app': '/path/to/your/app.apk',
        'appium:automationName': 'UiAutomator2',
    }],
    framework: 'mocha',
    reporters: ['spec', ['allure', {
        outputDir: 'allure-results',
        disableWebdriverStepsReporting: true,
        disableWebdriverScreenshotsReporting: false,
    }]],
    mochaOpts: {
        ui: 'bdd',
        timeout: 60000
    },
    afterTest: function (test, context, { error }) {
        if (error) {
            browser.takeScreenshot();
        }
    }
}
```

### 4. package.json
O arquivo package.json deve incluir as dependências e scripts necessários:

```json
{
  "name": "mobile-app-tests",
  "version": "1.0.0",
  "description": "Testes automatizados para aplicativo mobile",
  "main": "index.js",
  "scripts": {
    "test": "wdio run wdio.conf.js"
  },
  "dependencies": {
    "@wdio/appium-service": "^7.16.10",
    "@wdio/cli": "^7.16.10",
    "@wdio/local-runner": "^7.16.10",
    "@wdio/mocha-framework": "^7.16.10",
    "@wdio/spec-reporter": "^7.16.10",
    "@wdio/allure-reporter": "^7.16.10",
    "appium": "^1.22.0",
    "chai": "^4.3.4"
  }
}
```

### 5. .gitlab-ci.yml
Arquivo para configurar a integração contínua no GitLab:

```yaml
stages:
  - test

test:
  image: node:12
  script:
    - npm install
    - npm run test
  artifacts:
    when: always
    paths:
      - allure-results/
  only:
    - master
```

### 6. login.page.js
Exemplo de Page Object para a tela de login:

```javascript
class LoginPage {
    get inputEmail() { return $('#email'); }
    get inputPassword() { return $('#password'); }
    get btnLogin() { return $('#login-button'); }

    login(email, password) {
        this.inputEmail.setValue(email);
        this.inputPassword.setValue(password);
        this.btnLogin.click();
    }
}

module.exports = new LoginPage();
```

### 7. login.spec.js
Especificação de testes para login:

```javascript
const LoginPage = require('../pageobjects/login.page');
const loginData = require('../data/loginData.json');

describe('Testes de Login', () => {
    loginData.forEach(({ email, password, expected }) => {
        it(`Deve validar login para ${email}`, () => {
            LoginPage.login(email, password);
            const alertText = browser.getAlertText();
            expect(alertText).toEqual(expected);
        });
    });
});
```
