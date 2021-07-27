---
layout: default
---

<h1>Fundamentos de Android</h1>
<p>Nesse tópico serão apresentados os principais componentes de uma aplicação Android.</p>
 
<br>
<h2>Pontos de Entrada</h2>
<p align="justify">Saber quais são os pontos de entradas de uma aplicação é super importante para realizar a engenharia reversa, 
	pois é através deles que usuários e o próprio sistema interagem e acessam os aplicativos. 
	Em desenvolvimento Android existem quatro componentes principais:</p>

<br>
<h3>Activities</h3>
<p align="justify">Activities são consideradas a camada de apresentação dos aplicativos. Praticamente toda a interface da aplicação é construída em torno desse componente, definindo o layout e respostas à interações com o usuário.</p>
 
<p style="text-align:center;"><img src="./images/forest-app.jpg" width="450" height="300" /></p>
<h6 align="center">Exemplo de activity numa aplicação</h6>
  
   ```java
   	public class MainActivity extends AppCompatActivity {
			@Override
			protected void onCreate(Bundle savedInstanceState) {
				super.onCreate(savedInstanceState);
				setContentView(R.layout.activity_main);
			}
		}
   ```
<h6 align="center">Criação de uma activity.java</h6>
<br>
<p align="justify">Quando existem mais de uma activity em uma mesma aplicação, uma delas precisa ser definida, no arquivo de manifesto como aquela que será apresentada quando o aplicativo iniciar:</p>

```xml
	<code><activity android:name=".InitialActivity">
	  	<intent-filter>
    	  		<action android:name="android.intent.action.MAIN" />
        		<category android:name="android.intent.category.LAUNCHER" />
    		</intent-filter>
  	</activity></code>
```
<h6 align="center">Declaração de activity launcher no AndroidManifest</h6>

<br>
<h3>Services</h3>
<p align="justify">Services são os trabalhadores invisíveis das aplicações, uma vez que executam tarefas em background sem apresentar uma interface de interação com o usuário. Tocar música enquanto o usuário utiliza outros aplicativos, buscar dados na internet sem bloquear a interação do usuário com uma activity e sincronizar dados da aplicação são exemplos de services.</p>

```xml
	<service 
		android:name=".MyService"
         	android:exported="true">
	</service>
```
<h6 align="center">Declaração de um service no AndroidManifest.</h6>

<br>
<h3>Content Providers</h3>

<br>
<h3>Broadcast Receivers</h3>

<br>
<h2>Android Manifest</h2>
  <p align="justify">Toda aplicação Android possui um arquivo de manifesto, o <code class="language-plaintext highlighter-rouge">AndroidManifest.xml</code>, que fica na raíz do diretório do projeto e que define toda a estrutura, metadados, permissões, recursos de software e hardware necessários, bibliotecas utilizadas e componentes do aplicativo.</p>
  <p align="justify">Por ser um arquivo .xml sua estrutura é construída em tags, que pode ser entendido como blocos. A tag principal e mais externa contém informaçẽs mais gerais do aplicativo como nome, versão, ícone e tema, em seguida são utilizadas tags para definfir algumas configurações e por fim, a declaração dos componentes.</p>
  
  ```xml
	<manifest xmlns:android="http://schemas.android.com/apk/res/android"
		package="com.paad.myapp"
		android:versionCode="1"
		android:versionName="0.9 Beta"
		android:installLocation="preferExternal">
		
		<permission android: name=”com.paad.DETONATE_DEVICE”
            		android:protectionLevel=“dangerous”
            		android:label=”Self Destruct”>
		</permission>
		
		<application android:icon="@drawable/app_icon.png">
        		<activity android:name="com.example.project.ExampleActivity"
                  		android:label="@string/example_label">
        		</activity>
    		</application>
	</manifest>
  ```
  <h6 align="center">Exemplo de um AndroidManifest.xml</h6>
  
<br>
<h2>.APK</h2>
<p align="justify">Todo aplicativo Android é do tipo .apk, que basicamente é um arquivo .zip e sim, é possível descompactar um .apk para ver seu conteúdo. Ao descompactar um aplicativo muito provavelmente você verá as seguintes pastas:</p>

<dl>
	<dt>META-INF/</dt>
	<dd>Informações de manifesto e outros metadados usadas para executar as aplicações o pacote Java e o certificado usado para assinar o conteúdo.</dd>
	<br><dt>classes.dex</dt>
	<dd>Dalvik bytecode para a aplicação, no formato DEX. Esse é o código que executará por padrão.</dd>
	<br><dt>lib/</dt>
	<dd>Bibliotecas nativas da aplicação.</dd>
	<br><dt>assets/ ou res/</dt>
	<dd>Outros arquivos quaisqueres utilizados pela aplicação, geralmente estão ligados a UI.</dd>
	<br><dt>AndroidManifest.xml</dt>
	<dd>Metadados da aplicação.</dd>
</dl>
  
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
