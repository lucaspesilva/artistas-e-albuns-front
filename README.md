# Front de Artistas de Álbuns
Projeto prático de frontend do processo seletivo da Polícia Judiciária Civil de MT

Candidado: Lucas Pinto e Silva

### Dependências do Projeto
- Vue 2
- Vue-CLI 4
- Vue Router
- Vuetify
- Axios
- Projeto backend: https://github.com/lucaspesilva/artistas-e-albuns-api

### Como rodar a aplicação
1. Instalar o Node.js: https://nodejs.org/en/
2. Instalar o Docker: https://docs.docker.com/get-docker/
3. Clonar o repositório
4. Baixar as dependências:
```
npm install
```
5. Compilar o projeto:
```
npm run build
```
6. Criar a imagem do Docker:
```
docker build -t docker/artistas-e-albuns-front-image .
```
7. Rodar a aplicação no Docker (atentar para o mapeamento da porta 3000):
```
docker run -it -p 3000:8080 --rm --name artistas-e-albuns-front-image docker/artistas-e-albuns-front-image
```

### Como realizar os testes
1. Rodar o projeto backend na URL http://localhost:8080
2. Acessar o front pela URL http://localhost:3000
3. Realizar o login (por padrão, usuário "user" e senha "123")
4. Visualizar os álbuns cadastrados com paginação
5. Clicar no ícone de lupa para ver as capas do álbum

Obs: O cadastro de artistas, álbuns e envio de capas deve ser feito chamando a API de backend diretamente (por exemplo, pelo Swagger)