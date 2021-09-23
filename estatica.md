---
layout: default
---

## Análise Estática
<p align="justify">A análise estática está diretamente ligada à análise do código fonte da aplicação. Contudo, realisticamente, aplicativos Android podem ser extremamente grandes e, muito provavelmente, não será possível que essa análise seja realizada em todas as linhas de código. Por isso, neste tópico, falaremos sobre por onde começar e quais os pontos em que você deve ficar atento.</p>
<br><br>

<h2>Por onde começar?</h2>

- <h3>Saiba o seu objetivo</h3>
<p align="justify">Antes de começar a colocar a mão na massa, é preciso entender quais funcionalidades são esperadas de um aplicativo legítimo. Como muitos malwares tentam se passar por aplicações reais, é preciso primeiro identificar o que é proposto nas suas funções e qual o contexto da aplicação, para que seja possível determinar, então, se existe algum comportamento malicioso no aplicativo. Uma busca rápida na internet ou até mesmo na loja de apps já é suficiente para entender qual o contexto com o qual estamos lidando.</p> 
<p align="justify">O objetivo que devemos ter em mente é a busca por funcionalidades divergentes do contexto da aplicação. Por exemplo, um aplicativo para mudança de wallpaper não deveria realizar ações como enviar mensagens SMS ou obter a localização do dispositivo por GPS. Caso for possível acessar funcionalidades fora do contexto definido, isso pode ser uma indicação de que não se trata de uma aplicação legítima.</p>
<br>

- <h3><a href="https://github.com/MobSF/Mobile-Security-Framework-MobSF">MobSF</a></h3>
<p align="justify">É uma ferramenta de análise automatizada para aplicações mobile - APK, XAPK, IPA & APPX. Com essa ferramenta é possível fazer uma triagem rápida sobre as principais informações do aplicativo, ou também optar por uma abordagem mais aprofundada, realizando tanto análise estática quanto dinâmica.</p>

<p align="justify">Com o MobSF é possível fazer revisões do código, detectar permissões e configurações e verificar os mais diversos comportamentos como chamadas de API, componentes do tipo <i>browsable</i> e armazenamento inseguro. São muitas as possibilidades!</p>

<p style="text-align:center;"><img src="./images/mobsf.png" width="800" height="500"/></p>
<h6 align="center">MobSF após análise de APK</h6>

<p align="justify">Para saber mais, acesse: <a href="https://www.redbelt.com.br/blog/2021/04/29/automatizando-enumeracao-em-mobile-mobsf/">Automatizando Enumeração em Mobile</a></p>
<br>

- <h3>Permissões</h3>
<p align="justify"> Permissões são utilizadas para garantir alguns comportamentos e limitações dos componentes de uma aplicação, podendo indicar quais outros aplicativos podem se conectar e ações que ele pode realizar, muitas vezes permitindo que aplicativos executem ações maliciosas. Por exemplo, um aplicativo de música não deveria ter permissão para acessar os arquivos e a câmera do celular.</p>
<p align="justify">As permissões do Android estão classificadas em quatro níveis diferentes:</p>
  
| Nível da Permissão| Comentários                                                                      | 
|:------------------|:---------------------------------------------------------------------------------|
| Normal            | Não podem causar dano ao usuário.                                                |
| Dangerous         | Podem causar dano ao usuário.                                                    |
| Signature         | Garante a comunicação entre aplicativos assinados com o mesmo certificado.       | 
| Signature/System  | O mesmo que Signature, mas foi projetado apenas para fabricantes de dispositivos.|
  

<br>
<p align="justify">Algumas <a href="https://developer.android.com/reference/android/Manifest.permission">permissões</a> que podem indicar comportamento malicioso:</p>

  - android.permission.ACCESS_NETWORK_STATE
  - android.permission.INTERNET
  - android.permission.WRITE_EXTERNAL_STORAGE
  - android.permission.READ_EXTERNAL_STORAGE
  - android.permission.READ_PHONE_STATE
  - android.permission.CHANGE_WIFI_STATE
  - android.permission.RECEIVE_BOOT_COMPLETED
  - android.permission.ACCESS_MEDIA_LOCATION
  - android.permission.SEND_SMS
  - android.permission.CAMERA
  - android.permission.VIBRATE
  - android.permission.REQUEST_INSTALL_PACKAGES
<br>

- <h3>Chamadas de API</h3>
<p align="justify">Devemos ficar atentos às chamadas de <a href="https://www.redhat.com/pt-br/topics/api/what-are-application-programming-interfaces">API</a>. Ações como enviar ou receber SMS ou ligações, tirar print da tela, gravar vídeos ou áudios, podem ser usadas por alguns aplicativos como gatilhos para executar ações maliciosas e que, muitas vezes, nem sequer fazem sentido com a funcionalidade legítima que a aplicação se propõe.</p>
<br>

- <h3>Strings</h3>
<p align="justify">É comum encontrarmos escritas em lingaguem natural no código fonte que estamos analisando, essas escritas (também chamadas de strings) podem nos ajudar a entender como o aplicativo funciona. No arquivo strings.xml podemos visualizar todas as escritas que podem aparecer para o usuário, além disso, tente se manter atento a qualquer indício de escrita em lingua natural como nomes de funções, variáveis, emails e etc.</p>
<br><br>

<h2>Mas e a engenharia reversa?</h2>
<p align="justify">Não é sempre que temos acesso ao código fonte dos aplicativos. Então, como podemos obtê-lo para realizar sua análise? A resposta é simples: através da engenharia reversa. Como visto anteriormente, a engenharia reversa "desmonta" um objeto, permitindo com que seus componentes e funcionalidades sejam analisados. No nosso contexto, o objeto a ser analisado é um APK e, ao desmontá-lo, obtemos seu código fonte.</p>
<p align="justify">Existem diversas ferramentas que auxiliam na engenharia reversa. Aqui, vamos focar em duas.</p>
<br>

- <h3>unzip + dex2jar</h3>
<p align="justify">Como dito anteriormente, um arquivo .APK pode ser descompactado como um .zip. Porém, os arquivos gerados após esse processo não são de fácil entendimento por encontrar-se em DEX bytecode. Por isso, é preciso fazer a conversão para uma linguagem mais próxima da linguagem natural humana, como o Java.</p>

<p align="justify">Arquivos <a href="https://www.ti-enxame.com/pt/android/quais-sao-os-arquivos-.dex-no-android/939829692/">.dex</a> são um tipo de executável para a <a href="https://pt.wikipedia.org/wiki/Dalvik_virtual_machine">máquina virtual Dalvik</a> e incluem o código compilado que localiza e inicializa outros arquivos de programas necessários para executar o aplicativo.</p>

<p style="text-align:center;"><img src="./images/ReversersFlow.jpg" width="500" height="200"></p>
<h6 align="center">Fluxo de Engenharia reversa</h6>

<p align="justify">Assim como mostra a imagem, é preciso converter o DEX bytecode em SMALI e em seguida converter para JAVA. Uma das formas de se realizar essas conversões é utilizando o programa <a href="https://github.com/pxb1988/dex2jar/">dex2jar</a>.</p>
<br>

- <h3>jadx</h3>
<p align="justify">O <a href="https://github.com/skylot/jadx">jadx</a> é uma ferramenta de engenharia reversa que tem como objetivo produzir código Java a partir de arquivos .apk ou .dex. Além disso, também conta com um desofuscador de código, podendo ser muito útil para alguns casos, tornando o código muito mais legível.</p>

<p style="text-align:center;"><img src="https://raw.githubusercontent.com/skylot/jadx/master/jadx-gui/src/main/resources/logos/jadx-logo.png" width="200" height="200"></p>
<h6 align="center">Logo - jadx</h6>

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



