---
layout: default
---

## Análise Estática
<p align="justify">A análise estática está diretamente ligada à análise do código fonte da aplicação, porém aplicativos Android podem ser realisticamente grandes e muito provavelmente não será possível que executar essa análise em todas as linhas de código. Por isso, nesse tópico falaremos sobre por onde começar e quais os pontos em que você deve ficar atento.</p>
<br><br>

<h2>Por onde começar?</h2>

- <h3>Saiba o seu objetivo</h3>
<p align="justify">Antes de começar a colocar a mão na massa, saiba quais as funcionalidades esperadas para o aplicativo legítimo. Como muitos malwares tentam se passar por aplicações reais, é preciso primeiro identificar o que é proposto nas suas funções para que seja possível determinar quais os comportamentos maliciosos estamos buscando. Uma busca rápida na internet ou até mesmo na loja de apps já é suficiente para entender qual é o contexto com o qual estamos lidando.</p> 
<p align="justify">Devemos ter como objetivo a busca por funcionalidades divergentes da aplicação legítima. Por exemplo, um aplicativo para mudança de wallpaper não deveria realizar ações como envio de SMS ou de obter a localização do dispositivo por GPS.</p>
<br>

- <h3><a href="https://www.virustotal.com/gui/home/upload">Virus Total</a></h3>
<p align="justify">O Virus Total é uma plataforma online que analisa arquivos e URLs na tentativa da identificação de malwares. São utilizados banco de dados de antivírus parceiros para determinar se o artefato analisado é mal-intencionado ou não, usando também como resultado a forma como esses vários antivírus realizam as suas classificações.</p>
<p align="justify">Apesar dele não ter a função de um antivírus, seu uso pode ser vantajoso quando falamos de pré-análises, sendo útil para nos dar uma ideia se estamos realmente trabalhando com um malware.</p>

<p style="text-align:center;"><img src="./images/virustotal.png" width="400"></p>
<h6 align="center">Logo - Virus Total</h6>
<br>

- <h3>Chamadas de API</h3>
<p align="justify">Devemos ficar atentos à chamadas de <a href="https://www.redhat.com/pt-br/topics/api/what-are-application-programming-interfaces">API</a>, alguns aplicativos podem usar envio e/ou roubo de SMS, print da tela, gravador de som e câmera para executar ações maliciosas que muitas vezes nem sequer fazem sentido com a funcionalidade legítima que a aplicação se propõe.</p>
<br>

- <h3>Strings</h3>
<p align="justify">É comum encontrarmos escritas em lingaguem natural no código fonte que estamos analisando, essas escritas (também chamadas de strings) podem nos ajudar a entender como o aplicativo funciona. No arquivo strings.xml podemos visualizar todas as escritas que podem aparecer para o usuário, além disso, tente se manter atento a qualquer indício de escrita em lingua natural como nomes de funções, variáveis, emails e etc.</p>
<br>

<h2>unzip + dex2jar</h2>
<p align="justify">Como dito anteriormente, um arquivo .APK pode ser descompactado como um .zip, porém os arquivos gerados após esse processo não são de fácil entendimento pois encontram-se em DEX bytecode, e é preciso que sejam convertidos em uma linguagem mais próxima da linguagem natural humana, como o Java.</p>

<p align="justify">Arquivos <a href="https://www.ti-enxame.com/pt/android/quais-sao-os-arquivos-.dex-no-android/939829692/">.dex</a> são usados para inicializar e executar aplicativos desenvolvidos para Android. Os dados armazenados nesses arquivos inclui código compilado que localiza e inicializa outros arquivos de programas necessários para executar o aplicativo. O .dex é um executável da máquina virtual Dalvik e aplicações Java podem ser traduzidas em programas Android com arquivos executáveis associados. Vários arquivos DEX são armazenados em um pacote de distribuição e salvo no formato APK.</p>

<p style="text-align:center;"><img src="./images/ReversersFlow.jpg" width="800" height="500"></p>
<h6 align="center">Fluxo de Engenharia reversa</h6>

<p align="justify">Assim como mostra a imagem, é preciso converter o dex bytecode em SMALI e em seguida converter para JAVA. Podemos realizar essas conversões utilizando o programa <a href="https://tools.kali.org/reverse-engineering/dex2jar">dex2jar</a>.</p>
<br><br>

<h2>jadx</h2>
<p align="justify"></p>

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



