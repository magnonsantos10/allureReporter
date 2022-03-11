# allureReporter com Protractor
Relatório de automação com o Framework Allure Reporter

## Instalação do jasmine-allure-reporter

```
npm install jasmine-allure-reporter --save-dev
```
![image](https://user-images.githubusercontent.com/91263334/134576654-c58aac84-d0af-4b55-8472-d6d2428a0180.png)


Após instalação devemos adicionar o código abaixo no nosso arquivo de configuração do projeto para que o XML do relatório passe a ser gerado.
```
exports.config = {
  framework: 'jasmine2',
  onPrepare: function() {
    var AllureReporter = require('jasmine-allure-reporter')
    jasmine.getEnv().addReporter(new AllureReporter({
      resultsDir: 'allure-results'
    }))
  }
};
```

**Informativo:** Precisamos adicionar uma `function` ao `jasmineNodeOpts` conforme imagem abaixo:

![image](https://user-images.githubusercontent.com/91263334/134577198-9b0137a9-34d3-4723-90b7-6fbed37b0b61.png)

E enfim para abrir o relatório no navegador precisamos fazer a instalação da dependência do allure-commandline.

```
npm install allure-commandline --save-dev
```
![image](https://user-images.githubusercontent.com/91263334/134577421-52c471fb-0ba3-4eac-bb6b-88c96960d385.png)

Feito isso habilitamos o allure para linha de comando no nosso projeto.

Para gerar o relatório siga os comandos abaixo:

Esse comando faz com que seja gerado um relatório novo e com que qualquer outro antigo seja apagado na pasta padrão com o nome `allure-report`
```
allure generate --clean
```
Feito isso rodar o seguinte comando:

```
allure serve
```

Então o relatório será apresentado no seu navegador como esperado.

Foto ilustrativa:

![image](https://user-images.githubusercontent.com/91263334/134578034-25d3af2f-8859-4ea9-93cb-25c6fcf7580a.png)

**Informativo:** Para não ficar toda vez executando os comandos de limpeza do relatório, podemos adicionar esta tarefa no nosso script de testes no arquivo do `package.json`

Adicionar a linha abaixo ao arquivo `package-json`

```
"posttest": "allure generate allure-results --clean -o allure-report || true"
```

# Screenshots

Para que as screenshots sejam devidamente apresentadas no allure reporter devemos realizar as devidas configurações:

Dentro do `onPrepare` no arquivo de configuração, fazer a substituição do código anterior para o código abaixo:

```
onPrepare : function ( )   { 
    var  AllureReporter  = require ( ' jasmine-allure-reporter ' )
    
    jasmine.getEnv().addReporter(new AllureReporter());
        jasmine.getEnv().afterEach(function (done) {
            browser.takeScreenshot().then(function (png) {
                allure.createAttachment('Screenshot', function () {
                    return new Buffer(png, 'base64')
                }, 'image/png')();
                done();
            })
        });
```







