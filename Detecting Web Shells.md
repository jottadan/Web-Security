# Objetivo
Saber como detectar web shells.

# Overview
**Web Shells** são programas maliciosos enviados para um servidor web que permitem ao atacante executar comandos remotamente através de uma aplicação ou interface web. Podem ser utilizados tanto para acesso inicial quanto para persistência após o comprometimento.

Geralmente são implantados explorando vulnerabilidades de upload de arquivos, configurações inseguras ou acesso prévio ao servidor. Uma vez instalado, o web shell pode permitir reconhecimento, execução de comandos, escalação de privilégios, movimento lateral e exfiltração de dados.

**Exemplo:** um atacante explora um upload de imagens mal protegido para enviar um arquivo `.php` ou `.aspx` contendo um web shell e passa a controlar o servidor remotamente.

---

# Anatomia de um web shell
Web shells dependem de funções legítimas da linguagem ou do sistema, mas são abusadas para executar comandos maliciosos. Em PHP, funções como shell_exec(), exec(), system() e passthru() podem permitir a execução de comandos no servidor.

---

# Logs
Pode ser realizada principalmente através da análise e correlação de web server logs, Auditd e SIEM.

Nos **web server logs**, é importante procurar padrões anormais como requisições `GET`/`POST` repetidas, uso suspeito de métodos como `PUT`, acesso repetido ao mesmo arquivo, User-Agents suspeitos, IPs externos, query strings contendo parâmetros como `cmd=` ou `exec=`, strings codificadas e ausência de referrer. Esses indicadores devem ser analisados em conjunto, pois isoladamente podem ter explicações legítimas.

O **Auditd** fornece evidências no nível do sistema operacional, permitindo identificar criação, modificação e execução de arquivos, além do usuário ou processo envolvido.

A **correlação entre web logs e Auditd** permite reconstruir melhor o ataque. Por exemplo, um `POST` suspeito pode ser relacionado a um evento `creat`, indicando que uma requisição web levou à criação ou execução de um arquivo.

Um **SIEM** facilita esse processo ao centralizar logs, permitir consultas direcionadas e correlacionar eventos de diferentes fontes.

# Análise de arquivos

Ao analisar o sistema de arquivos, o objetivo é identificar arquivos suspeitos ou recentemente modificados em diretórios web, como `/var/www/html/` e `/usr/share/nginx/html/`. Sendo importante procurar **extensões executáveis, nomes aleatórios, double extensions** como `image.jpg.php` e funções suspeitas como `eval()`.

Ao análisar o tráfego de rede, o objetivo é identificar requisições e comportamentos anormais, como **métodos HTTP incomuns, User-Agents suspeitos, payloads codificados, comandos em requisições e uploads de arquivos maliciosos**. O Wireshark pode ser usado para analisar esses eventos em detalhes.

A correlação entre **logs, sistema de arquivos e tráfego de rede** fornece evidências mais fortes para confirmar a presença e a atividade de um web shell.

---

# Desafio do lab
1. *Qual o IP do atacante?*
2. *Qual é o primeiro diretório que o atacante identifica?*
3. *Qual o nome do arquivo .php que o atacante usa para fazer o upload do web shell?*
4. *Qual é o primeiro comando executado pelo atacante usando o web shell?*
5. *Após obter acesso via web shell, o atacante utiliza um comando para baixar um segundo arquivo para o servidor. Qual é o nome desse arquivo?*
6. *Encontre o segredo que está escondindo junto do web shell*
