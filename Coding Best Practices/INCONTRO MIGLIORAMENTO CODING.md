
___

### [[TDD (Test Driven Development)]]

---
### [[Metodologia AGILE, Agile Software Development (ASD)]]

---
### [[LINT]]

---
### [[Model View Controller]]

---

### [[Unit Test]]

---

### DRY (Don't Repeat Yourself)

---

### [[Principi programmazione ad oggetti o Object Oriented Programming (OOD)]]

e

### [[SOLID]]


best practices -> puntre su qualità:
- focus: 
- delivery: riuscire a fornire cosa
- optimization: anticipare i bisogni

interface segregazione -> se non ce bisogno di un metodo che è ereditato, si sta sbagliando qualcosa. classi riutilizzate che però non funzionano.

COME USARE UN METODO PRIVATO

Codice MyWelder
semafori, flag che indicano i processi in corso e terminati falliti o con successo. 


- [ ] clangd -> https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-clangd

---


obiettivo dell'incontro:  follow up della prima giornata. Presentazione dei corsi suggeriti e metodologia proposta

  

1. [linter](https://github.com/cpplint/cpplint "https://github.com/cpplint/cpplint") per C++ con [guide style](https://google.github.io/styleguide/cppguide.html "https://google.github.io/styleguide/cppguide.html") di google, come esempi ma dipende anche da che IDE utilizzate (se usaste VS code potreste usare [questa](https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-clangd "https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-clangd") estensione);  
    
2. [corso](https://www.udemy.com/course/beg-modern-cpp/ "https://www.udemy.com/course/beg-modern-cpp/") di C++ con parte per OOP;
3. [corso](https://www.udemy.com/course/writing-clean-code/ "https://www.udemy.com/course/writing-clean-code/") di clean code con C++;
4. [corso](https://www.udemy.com/course/cplusplus-unit-testing-google-test-and-google-mock/ "https://www.udemy.com/course/cplusplus-unit-testing-google-test-and-google-mock/") per unit testing con C++.

test -> concetto di isolamento
concetto di mocking delle parti che non sono sotto test (implemento contratto che si comporta come il sistema in cui eseguo il test)

SOLID 5 principi che rendono il codice iù mantenibile, flessibile, testabile e si basano su polimorfismo e dividi e impera

dependancy invertion/injection una delle basi del principi del solid, nel costruttore definisco le dipendenze (parametri) per fare cio che deve.

ho una classe che ha serie di azioni grazie a metodi. non interessata da elementi fuori dal suo dominio. 

una service sono classi che sono nel costruttore che operano sul dominio.

---
`16/11/2023`
meeting per corsi di formazione coding

