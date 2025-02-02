### Este arquivo apresenta os scripts de criação das tabelas de armazenamento dos dados coletados pela ferramenta do Observatório do Consumidor
##### Autor: Thallys da Silva Nogueira

### Tabelas Específicas do X
#### Esta tabela é responsável por armazenar informações sobre as palavras-chave referentes aos nomes dos produtos
~~~
CREATE TABLE produto(id_produto INT AUTO_INCREMENT PRIMARY KEY,
nome_produto VARCHAR(50),
categoria_produto VARCHAR(50),
sub_categoria1 TEXT,
sub_categoria2 TEXT,
estado VARCHAR(50))
~~~

#### Esta tabela é responsável por armazenar informações dos usuários do X que mencionaram alguma das palavras-chave referentes aos nomes dos produtos em suas publicações
~~~
CREATE TABLE usuario(user_id_str VARCHAR(255) PRIMARY KEY NOT NULL,
user_name TEXT,
user_screen_name TEXT,
user_location TEXT,
user_url TEXT,
user_description TEXT,
sexo TEXT,
idade_media INT,
profissao TEXT,
classe_renda TEXT,
user_followers_count INT,
user_friends_count INT,
user_listed_count INT,
user_favourites_count INT,
user_statuses_count INT,
user_created_at TEXT,
user_profile_image_url TEXT,
user_profile_image_url_https TEXT,
user_following TEXT,
retweeted_status_userid TEXT,
retweeted_status_username TEXT)
~~~

#### Esta tabela é responsável por armazenar as publicações dos usuários do X que mencionaram alguma das palavras-chave referentes aos nomes dos produtos em seus tweets
~~~
CREATE TABLE tweet(id_tweet INT AUTO_INCREMENT PRIMARY KEY, user_id_str VARCHAR(255),
id_produto INT,
created_at TEXT,
id_str TEXT,
text TEXT,
source TEXT,
in_reply_to_status_id_str TEXT,
in_reply_to_screen_name TEXT,
coordinates_lng TEXT,
coordinates_lat TEXT,
retweeted_status TEXT,
retweet_count INT,
favorite_count INT,
entities_hashtags TEXT,
entities_user_mentions TEXT,
entities_user_mentions_id_str TEXT,
entities_user_mentions_screen_name TEXT,
data_mineracao TEXT,
FOREIGN KEY (user_id_str) REFERENCES usuario(user_id_str),
FOREIGN KEY (id_produto) REFERENCES produto(id_produto))
~~~

#### Esta tabela é responsável por armazenar as informações dos estados brasileiros
~~~
CREATE TABLE estado(id_estado INT AUTO_INCREMENT PRIMARY KEY,
nome_estado TEXT,
localizacao TEXT)
~~~

#### Esta tabela é responsável por armazenar as informações das cidades brasileiras
~~~
CREATE TABLE cidade(id_cidade INT AUTO_INCREMENT PRIMARY KEY,
nome_cidade TEXT,
id_estado int,
FOREIGN KEY (id_estado) REFERENCES estado(id_estado))
~~~

#### Esta tabela é responsável por armazenar as informações processadas das regiões do Brasil da análise de georeferenciamento
~~~
CREATE TABLE georeferencia(id_localizacao INT AUTO_INCREMENT PRIMARY KEY,
user_id_str VARCHAR(255),
id_tweet INT,
id_estado INT,
localizacao TEXT,
FOREIGN KEY (user_id_str) REFERENCES usuario(user_id_str),
FOREIGN KEY (id_tweet) REFERENCES tweet(id_tweet),
FOREIGN KEY (id_estado) REFERENCES estado(id_estado))
~~~

#### Esta tabela é responsável por armazenar as informações sobre possíveis profissões dos usuarios mineradas do campo user_description
~~~
CREATE TABLE ocupacaoBrasileiros(id_ocupacao INT AUTO_INCREMENT PRIMARY KEY,
cbo INT,
palavra_chave TEXT,
cargo TEXT,
carga_horaria TEXT,
piso_salarial TEXT,
media_salarial TEXT,
salario_mediano TEXT,
teto_salarial TEXT,
salario_hora TEXT,
classe_renda TEXT)
~~~

### Tabelas Específicas do Youtube

#### Esta tabela é responsável por armazenar informações sobre as palavras-chave referentes aos nomes dos produtos
~~~
CREATE TABLE produto(id_produto INT AUTO_INCREMENT PRIMARY KEY,
nome_produto VARCHAR(50))
~~~

#### Esta tabela é responsável por armazenar informações dos vídeos coletados que mencionaram as palavras-chave referentes aos nomes dos produtos 
~~~
CREATE TABLE youtubeVideos(id_video INT AUTO_INCREMENT PRIMARY KEY,
id_video_url TEXT,
palavra_chave TEXT,
titulo TEXT,
url_video TEXT,
duracao_segundos INT,
qtd_visualizacoes INT,
qtd_curtidas INT,
qtd_comentarios INT,
data_publicacao TEXT,
autor TEXT)
~~~

#### Esta tabela é responsável por armazenar os comentários dos vídeos coletados que mencionaram as palavras-chave referentes aos nomes dos produtos 
~~~
CREATE TABLE comentariosYoutubeVideos(id_comentario INT AUTO_INCREMENT PRIMARY KEY,
id_video INT,
comentario TEXT,
FOREIGN KEY (id_video) REFERENCES youtubeVideos(id_video))
~~~

### Tabelas em comum entre as redes sociais do X e YouTube

#### Esta tabela é responsável por armazenar os resultados dos sentimentos das publicações do X e dos comentários dos vídeos coletados do YouTube que mencionaram as palavras-chave referentes aos nomes dos produtos 
~~~
CREATE TABLE sentimento(id_sentimento INT AUTO_INCREMENT PRIMARY KEY,
id_tweet INT,
sentimento INT,
FOREIGN KEY (id_tweet) REFERENCES tweet(id_tweet))
~~~

#### Esta tabela é responsável por armazenar as palavras-chave das tabelas TACO e POF utilizadas na mineração dos textos da publicações do X e dos comentários dos vídeos coletados do YouTube
~~~
CREATE TABLE tendencia(id_tendencia INT AUTO_INCREMENT PRIMARY KEY,
nome_caracteristica TEXT,
nome_tendencia TEXT,
categoria TEXT,
sub_categoria TEXT,
categoria_alimentar TEXT,
tabela_referencia TEXT)
~~~

#### Esta tabela é responsável por armazenar os resultados do processamento de mineração dos textos da publicações do X e dos comentários dos vídeos coletados do YouTube
~~~
CREATE TABLE opiniao(id_opniao INT AUTO_INCREMENT PRIMARY KEY,
id_tweet INT,
id_tendencia INT,
FOREIGN KEY (id_tweet) REFERENCES tweet(id_tweet),
FOREIGN KEY (id_tendencia) REFERENCES tendencia(id_tendencia))
~~~

#### Esta tabela é responsável por armazenar as palavras-chave dos adjetivos de interesse utilizadas na mineração dos textos da publicações do X e dos comentários dos vídeos coletados do YouTube
~~~
CREATE TABLE adjetivos(id_adjetivo INT AUTO_INCREMENT PRIMARY KEY,
adjetivo TEXT,
polaridade TEXT)
~~~

#### Esta tabela é responsável por armazenar as palavras-chave de verbos de interesse utilizados na mineração dos textos da publicações do X e dos comentários dos vídeos coletados do YouTube
~~~
CREATE TABLE comportamentoUsuario(id_verbo INT AUTO_INCREMENT PRIMARY KEY,
verbo TEXT,
categoria_verbo TEXT,
categoria_estilo_vida TEXT)
~~~

#### Esta tabela é responsável por armazenar os resultados das ações verbais de interesse encontradas nos textos da publicações do X e dos comentários dos vídeos coletados do YouTube
~~~
CREATE TABLE consumoLacteo(id_consumo_lacteo INT AUTO_INCREMENT PRIMARY KEY,
id_tweet INT,
verbo_infinitivo	TEXT,
verbo_mencionado TEXT,
produto_ligado_ao_verbo TEXT,
FOREIGN KEY (id_tweet) REFERENCES tweet(id_tweet))
~~~

#### Esta tabela é responsável por armazenar os resultados das qualidades dos produtos lácteos interesse encontradas nos textos da publicações do X e dos comentários dos vídeos coletados do YouTube
~~~
CREATE TABLE caracteristicasLacteos(id_caracteristicas_lacteos INT AUTO_INCREMENT PRIMARY KEY,
id_tweet INT,
produto TEXT,
adjetivo TEXT,
FOREIGN KEY (id_tweet) REFERENCES tweet(id_tweet))
~~~
