
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

- `/test`: Contém os scripts de teste.
- `/pageobjects`: Implementação do padrão Page Object.
- `/data`: Arquivos de dados para testes data-driven.
- `/screenshots`: Capturas de tela geradas após falhas nos testes.

## CI/CD

Este projeto está integrado ao GitLab CI/CD para execução automática dos testes após cada commit.
