# desafio backend-spring

link desafio 

Desafio de um processo seletivo antigo do IFood (2017) realizado praticar Java e REST, e conhecer novas tecnologias.

Repositório original: https://github.com/ifood/ifood-backend-basic-test

Encontrado em: https://github.com/CollabCodeTech/backend-challenges

Resumo
O desafio consiste em criar uma API capaz de se comunicar com uma API externa (OpenWeatherMapAPI) e retornar os dados climáticos de uma cidade a partir do seu nome. Além disso, deve se utilizar de cache para tornar o serviço mais rápido. Também precisa implementar controle de falhas para casos onde API externa não funcione corretamente.

Solução
A solução foi implementada com Spring Boot e está disponível no DockerHub andreepdias:

docker pull andreepdias/challenge-backend-ifood

Pode ser executada por:

docker run -p 8080:8080 andreepdias/challenge-backend-ifood

Endpoints
Existem três endpoints:

GET http://localhost:8080/weather/?city=joinville: retorna os dados climáticos da cidade (troque 'joinville' por outra cidade);

GET http://localhost:8080/weather/fake/?city=joinville: imita uma falha na requisição (com uma URL inexistente) para ativar o CircuitBreaker do controle de falhas;

GET http://localhost:8080/cache: retorna dados do gerenciador de cache (hits, misses, etc.).

Cache
A biblioteca escolhida para atuar como cache foi a Caffeine Cache, por ser uma biblioteca com uma alta performance e compatível com o contexto de cache do Spring Boot (e pelo nome ☕).

O tempo de expiração de um elemento no cache foi configurado como 10 minutos, por ser este o tempo que o serviço OpenWeatherAPI demora para atualizar os dados de temperatura atual de uma cidade. O número máximo de elementos no cache foi escolhido como 1000.

Controle de Falhas
O controle de falhas é implementado com o CircuitBreaker do Hystrix, na camada da serviço com um método de fallback que é ativado em caso de falha na comunicação com o serviço externo.

O tempo mínimo que o Hystrix deve esperar antes de abrir o circuito foi escolhido como 10s, devido ao fato do valor default ser mais baixo que o tempo de resposta do OpenWeatherAPI, em alguns casos.