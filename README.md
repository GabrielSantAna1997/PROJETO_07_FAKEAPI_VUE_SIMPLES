CRIAR O PROJETO:
1 - vue create .
2 - remover o linter e colocar o router.
3- 3x
4 - executar é npm run serve

CONECTAR COM O BACKEND API:
1 - npm install json-server
2- Em packege.json em scripts colocar: "backend": "json-server --watch db/db.json"
3- criar uma pasta 'db' e um arquivo 'db.json'.
4- colocar alguns dados na api.
5-executar : npm run backend ou rodar em outra porta json-server --watch db.json --port 8083 		