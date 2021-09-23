---
layout: default
---

<h1>Demonstração</h1>

<p align="justify">Nesse tópico iremos juntos analisar um malware real que tenta se passar por uma aplicação legítima. Passaremos por todas as etapas de uma análise real, fazendo uso da engenharia reversa. <i>Let's enjoy the ride!</i> </p>
  
<p align="justify">Antes de iniciarmos certifique-se de que tem todo o ambiente preparado aí na sua máquina!</p>
  
- Faça download do software <a href="https://www.virtualbox.org/wiki/Downloads">VirtualBox</a>;<br>
- Faça download da <a href="https://mega.nz/file/0VtyQbjS#tMZpaapBrAmZcqiYPpqZP7m7unH5lKKym7DI57PGKF0">máquina virtual</a> criada para esse minicurso;<br>
- Importe a máquina virtual para o VirtualBox. Caso precise de ajuda com isso, acesse o <a href="https://www.aplicativosandroid.com/como-importar-e-exportar-arquivos-ova-no-virtualbox/">link</a>.<br>
  
<br>
  
<h2>First things first</h2>
<p align="justify">O aplicativo que analisaremos já foi retirado há tempos da Play Store, então não é possível realizar a captura dos comportamentos esperados por esse meio, por isso mostraremos aqui como ele era apresentado para o publico em geral.</p>

<p align="justify">Vamos analisar um malware que ficou muito famoso em 2017 e que voltou com força em 2020 graças a pandemia. Ele se passava por um aplicativo informativo usando pessoas muito preocupadas com a COVID-19 e pouco informadas sobre malwares mobile como alvo de ataque.</p>
  
<p style="text-align:center;"><img src="./images/download.png"></p>
<h6 align="center">Site para download do aplicativo</h6>
<br>
  
<p align="justify">Usando uma campanha falsa sobre a COVID-19 voltada para a Turquia, conseguiu enganar e perssuadir muitas pessoas a instalarem o aplicativo em seus dispositivos, sendo possível realizar o download pelo site da foto ou pela Play Store.</p>
<br><br>
  
<h2>Analisando o comportamento</h2>
<p align="justify">Agora que já sabemos um pouco sobre a aplicação, podemos começar analisando seu comportamento dentro do ambiente crontolado da VM apresentada no ínicio desse tópico. Precisamos instalar o apk malicioso em um dispositivo emulado e testar as funcionalidades que ele apresenta para o usuário.</p>
<p align="justify">Para fazer isso, inicie a sua VM e abra o programa <a href="https://anbox.io/">Anbox</a>, localizado na barra lateral:</p>
  
<p style="text-align:center;"><img src="./images/Anbox_icone.png"></p>
<h6 align="center">Localização do Anbox na barra lateral</h6>

<br><br>

<p style="text-align:center;"><img src="./images/Anbox.png"></p>
<h6 align="center">Anbox após iniciar</h6>
  
<p align="justify">Esse é o nosso dispositivo Android emulado, funciona como um celular comum e podemos instalar aplicações maliciosas de qualquer tipo sem correr risco de sofrer ataques em nossas informações e documentos pessoais.</p>
  
<p align="justify">Vamos agora instalar o malware que está localizado na pasta Malware, na Àrea de Trabalho. Para isso, utilizaremos o <a href="https://developer.android.com/studio/command-line/adb?hl=pt-br&authuser=2">Android Debug Bridge</a>, que já foi mencionado anteriormente nesse minicurso. O ADB estabelece uma comunicação com dispositivos, sendo possível realizar os mais diversos testes, através de uma <a href="https://guialinux.uniriotec.br/shell/">shell</a> interativa.</p>
<br>
<p align="justify">Para instalar o malware no dispositivo emulado, abra um terminal na pasta Malware e digite o seguinte comando de instalação:</p>

  ```xml
  adb install pandemistek.apk
  ```
  
<p align="justify">Caso queira aprender mais comandos do ADB, acesse: <a href="https://www.automatetheplanet.com/wp-content/uploads/2019/08/Cheat_sheet_ADB.pdf">ADB cheat Sheet</a>.</p> 
<br><br>
  
<p align="justify">Agora é possível ver o ícone da aplicação instalado no emulador:</p>
  
<p style="text-align:center;"><img src="./images/malware_installed.png"></p>
<h6 align="center">Lista de aplicativos do emulador após intalação do malware</h6>
<br><br>

<p align="justify">Tudo pronto, podemos testar como o aplicativo funciona. Ao executar dando dois cliques na tela, vemos que um menu de configurações é aberto e é solicitado a habilitação da acessibilidade.</p>
  
<p style="text-align:center;"><img src="./images/accessibility_malware.png" width="700" height="500"></p>
<h6 align="center">Janela aberta após iniciar malware</h6>

<a href="https://.pngtree.com/so/Lâmpada'>Lâmpada png de .pngtree.com/"><img src="./images/lamp2.png" width="30" height="30"></a><span style="color:yellow"><strong>Para pensar:</strong></span><br>
  
   - O que acontece quando a acessibilidade é ativada? Por que isso acontece?<br>
   - Por que é pedido para ativar a acessibilidade?<br>


<h2>Capturando Logs</h2>
<p align="justify">Analisar e acompanhar os logs é muito útil para entender o funcionamento da aplicação. Logs são informações sobre a aplicação com o intuito de auxiliar um tester, essas informações estão ao longo do código e vão aparecendo de acordo com a autilização. Apesar de não sermos testers, podemos utilizar os logs para descobrir quais componentes estão sendo iniciados, valores de variáveis, entrada e saída de valores, comentários sobre o fluxo do código, entre outras coisas. </p>
<p align="justify">A primeira coisa a se fazer para capturar os logs é descobrir qual o nome do pacote da aplicação:</p>
 
 ```xml
adb shell pm list packages
  ```
  
<p align="justify">Certamente não apareceu nenhum pacote similiar ao nome do apk, pandemistek. Lembre-se de que estamos lidando com uma aplicação maliciosa que faz uso de todos os recursos disponíveis para se passar como uma aplicação legítima, inclusive a obfuscação no pacote da aplicação. Por isso, dada a lista de pacotes, procure pelos nomes que causam mais estranhamento. Nesse caso, o nome do pacote buscado é na verdade <code class="language-plaintext highlighter-rouge">naqsl.ebxcb.exu</code></p>
<p align="justify">Sabendo o nome da aplicação, devemos agora utilizar o seguinte comando para capturar os logs:</p>

 ```xml
adb logcat | grep naqsl.ebxcb.exu
  ```
  
<p align="justify">Dica: Caso queira acompanhar os logs durante o uso da aplicação, deixe o terminal aberto ao testar seu funcionamento.</p>
<br><br>

<h3>Sobre o MobSF</h3>
<p align="justify">Já falamos sobre o MobSF em tópicos anteriores e seria muito interessante se pudessemos utilizá-lo para verificar alguns pontos da aplicação, mas infelizmente não conseguimos instalar na VM por falta de espaço. Desse modo, não será possível o executarmos, porém, deixamos o relatório em PDF dentro da pasta Malware. Caso queira utilizar ou olhar só por curiosidade, fique à vontade.</p>

<br><br>  
<p align="justify">------- PASSOS DA PRÁTICA -------</p>

1. Instalar o malware no device: abra o terminal -> cd Área\ de\ Trabalho/APKs/ -> adb install pandemidestek.apk
2. Mostrar o funcionamento do malware.
3. Mostrar como pegar os logs: achar o package name com adb shell pm list package -> adb logcat | grep naqsl.ebxcb.exu
4. Mostrar relatório do MobSF.
5. Fazer engenharia reversa com o jadx.
6. Mostrar o AndroidManifest e destacar: permissões e componentes suspeitos, main activity.
7. Mostrar a main activity
8. Duas opções: deixar livre pra eles darem uma explorada nos componentes que quiserem e depois ir discutir OU escolher um componente suspeito, deixar uns 5 minutos pra eles olharem, e começar a discussão, repetindo pra todos os componentes que selecionarmos.
9. Finalizar com uma lista das funcionalidades maliciosas que podem ser encontradas no apk.
  
  
  
<br><br>
<hr />
<h3 align="right">Tópicos</h3>
<ul align="right">
<a href="https://darknenblack.github.io/RevEng-Android/">Engenharia Reversa</a><br>
<a href="https://darknenblack.github.io/RevEng-Android/fundamentos.html">Fundamentos de Android</a><br>
<a href="https://darknenblack.github.io/RevEng-Android/estatica.html">Análise Estática</a><br>
<a href="https://darknenblack.github.io/RevEng-Android/malware.html">Malwares</a><br>
<a href="https://darknenblack.github.io/RevEng-Android/demo.html">Demo</a><br>
<a href="https://darknenblack.github.io/RevEng-Android/ref.html">Referências</a><br>
</ul>
