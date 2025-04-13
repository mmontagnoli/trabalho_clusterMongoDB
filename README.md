# trabalho_clusterMongoDB
Repositório criado para armazenar todos os comandos utilizados para criação e validação de um servidor Cluster no MongoDB via Docker.

docker system prune --all  									->  Remove todos os contêineres, volumes, networks e afins.

docker pull mongodb/mongodb-community-server:latest 		-> Instala o mongoDB ao Docker (lastest)

docker network create mongoCluster							-> Cria uma docker network


 Criação de um contêiner:

docker run -d --rm -p 27017:27017 --name cluster10 --network mongoCluster mongodb/mongodb-community-server:latest --replSet myReplicaSet --bind_ip localhost,cluster10  

docker run -d --rm -p 27018:27017 --name cluster20 --network mongoCluster mongodb/mongodb-community-server:latest --replSet myReplicaSet --bind_ip localhost,cluster20

docker run -d --rm -p 27019:27017 --name cluster30 --network mongoCluster mongodb/mongodb-community-server:latest --replSet myReplicaSet --bind_ip localhost,cluster30

docker run -d --rm -p 27020:27017 --name cluster40 --network mongoCluster mongodb/mongodb-community-server:latest --replSet myReplicaSet --bind_ip localhost,cluster40


docker exec -it cluster10 mongosh 							-> Entrar dentro de um contêiner

db.runCommand ({hello:1})									-> Teste para ver se está conectado e executando corretamente



Criando um conjunto de réplicas
rs.initiate ({ _id: "myReplicaSet", members:[{_id:0, host: "cluster10"}, {_id:1, host: "cluster20"}, {_id:3, host: "cluster30"}, {_id:4, host: "cluster40"}]})	



docker exec -it cluster30 mongosh --eval "rs.status()"		-> Status do seu conjunto de réplicas, incluindo a lista de membros

docker exec -it cluster10 mongosh							-> entrar dentro de um contêiner e pegar a string

rs.isMaster().primary										-> validar qual nó que está 

use CompanySystem											-> criar um nome para o banco de dados



Comandos para inserir dados no banco de dados:

db.carro.insertOne({codigo:1, nome: "Corolla"}); 		
db.carro.insertOne({codigo:2, nome: "Civic"});
db.carro.insertOne({codigo:3, nome: "Pegeout 306"});   		
db.carro.insertOne({codigo:4, nome: "Fit"});
db.carro.insertOne({codigo:5, nome: "A3"});


db.carro.find()											-> verificar todos os dados inseridos

docker stop cluster10                                   -> para um nó do conjunto de réplicas
