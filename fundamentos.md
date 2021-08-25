---
layout: default
---

<h1>Fundamentos de Android</h1>
<p>Neste tópico, será apresentado como é composta uma aplicação Android e quais seus principais componentes.</p>

<br>
<h2>.APK</h2>
<p align="justify">Todo aplicativo Android é do tipo .apk. Basicamente, um arquivo .apk é um .zip e, assim como tal, é possível descompactá-lo para ver seu conteúdo. Ao descompactar um aplicativo, muito provavelmente você verá as seguintes pastas:</p>

<dl>
	<dt>META-INF/</dt>
	<dd>Informações de manifesto e outros metadados usadas para executar as aplicações, o pacote Java e o certificado usado para assinar o conteúdo.</dd>
	<br><dt>classes.dex</dt>
	<dd>Dalvik bytecode para a aplicação, no formato DEX. Esse é o código que executará por padrão.</dd>
	<br><dt>lib/</dt>
	<dd>Bibliotecas nativas da aplicação.</dd>
	<br><dt>assets/ ou res/</dt>
	<dd>Outros arquivos quaisquer utilizados pela aplicação. Geralmente, estão ligados à UI.</dd>
	<br><dt>AndroidManifest.xml</dt>
	<dd>Metadados da aplicação.</dd>
</dl>

<br>
<h2>Android Manifest</h2>
  <p align="justify">Toda aplicação Android possui um arquivo de manifesto, o <code class="language-plaintext highlighter-rouge">AndroidManifest.xml</code>. Ele fica na raíz do diretório do projeto e define toda a estrutura, metadados, permissões, recursos de software e hardware necessários, bibliotecas utilizadas e componentes do aplicativo.</p>
  <p align="justify">Por ser um arquivo .xml, sua estrutura é construída em tags, que podem ser entendidas como blocos. A tag principal e mais externa contém informações mais gerais do aplicativo, como nome, versão, ícone e tema. Dentro dela, são utilizadas tags para definir algumas configurações e, por fim, a declaração dos componentes.</p>
  
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
  <h6 align="center">Exemplo de um AndroidManifest.xml.</h6>
 
<br>
<h2>Pontos de Entrada</h2>
<p align="justify">Saber quais são os pontos de entradas de uma aplicação é super importante para realizar a engenharia reversa, 
	pois é através deles que usuários e o próprio sistema interagem e acessam os aplicativos. 
	Em desenvolvimento Android, existem quatro componentes principais:</p>

<br>
<h3>Activities</h3>
<p align="justify">Activities são consideradas as camadas de apresentação dos aplicativos. Praticamente toda a interface da aplicação é construída em torno desse componente, no qual é definido o layout e respostas às interações com o usuário.</p>
 
<p style="text-align:center;"><img src="./images/forest-app.jpg" width="450" height="300" /></p>
<h6 align="center">Exemplo de activity em uma aplicação.</h6>
  
   ```java
   	public class MainActivity extends AppCompatActivity {
			@Override
			protected void onCreate(Bundle savedInstanceState) {
				super.onCreate(savedInstanceState);
				setContentView(R.layout.activity_main);
			}
		}
   ```
<h6 align="center">Criação de uma activity.java.</h6>
<br>
<p align="justify">Quando existem mais de uma activity em uma mesma aplicação, uma delas precisa ser definida no arquivo de manifesto como aquela que será apresentada quando o aplicativo iniciar. Isso pode ser feito ao adicionar o seguinte intent filter (intent filters serão explicados mais à frente):</p>

```xml
	<activity android:name=".InitialActivity">
	  	<intent-filter>
    	  		<action android:name="android.intent.action.MAIN" />
        		<category android:name="android.intent.category.LAUNCHER" />
    		</intent-filter>
  	</activity>
```
<h6 align="center">Declaração de activity launcher no AndroidManifest.</h6>

<br>
<h3>Services</h3>
<p align="justify">Services são os trabalhadores invisíveis das aplicações, uma vez que executam tarefas em background sem apresentar uma interface de interação com o usuário. Tocar música enquanto o usuário utiliza outros aplicativos, buscar dados na internet sem bloquear a interação do usuário com uma activity e sincronizar dados da aplicação são exemplos de services.</p>

```java
	public class MyService extends Service {
		@Override
    		public void onCreate() {
        		super.onCreate();
    		}
	}
```
<h6 align="center">Criação de um service.java.</h6>

Services devem ser declarados no AndroidManifest.

```xml
	<service android:name=".MyService"
         	android:exported="false">
	</service>
```
<h6 align="center">Declaração de um service no AndroidManifest.</h6>

<br>
<h3>Content Providers</h3>
<p align="justify">Content Providers gerenciam a comunicação entre as aplicações e as bases de dados. Somente aplicações com as devidas permissões conseguem requisitar a um content provider acesso aos dados, podendo ler e alterar seu conteúdo.</p>

```java
	public class MyContentProvider extends ContentProvider {
		@Override
    		public void onCreate() {
        		super.onCreate();
    		}
	}
```
<h6 align="center">Criação de um content provider.</h6>

Assim como os services, content providers devem ser declarados no AndroidManifest.

```xml
	<provider android:name=".MyContentProvider"
         	android:exported="false">
	</provider>
```
<h6 align="center">Declaração de um content provider no AndroidManifest.</h6>

<br>
<h3>Broadcast Receivers</h3>
<p align="justify">Broadcast Receivers esperam por mensagens que indiquem que determinados eventos ocorreram. Essas mensagens podem ser enviadas tanto pelo sistema quanto por outros aplicativos. Por exemplo, um aplicativo fez o download de alguns dados e enviou uma mensagem para outros aplicativo para avisar que tais dados estão disponíveis para uso. Ou também, o sistema enviou uma mensagem avisando que a tela do dispositivo foi desligada.</p>

<p align="justify">Broadcast Receivers podem ser declarados tanto pelo AndroidManifest quanto dinamicamente. Quando declarados no AndroidManifest, deve ser definido que tipo de mensagens ele deve estar preparado para receber através de um intent filter.</p>

```java
	public class MyBroadcastReceiver extends BroadcastReceiver {
        	@Override
        	public void onReceive(Context context, Intent intent) {
        	}
    	}
```
<h6 align="center">Criação de um broadcast receiver.</h6>

```xml
	<receiver android:name=".MyBroadcastReceiver"  
		android:exported="true">
    		<intent-filter>
        		<action android:name="android.intent.action.BOOT_COMPLETED"/>
    		</intent-filter>
	</receiver>
```
<h6 align="center">Declaração de um broadcast receiver no AndroidManifest.</h6>

<p align="justify">Para declarar um broadcast receiver dinamicamente, primeiro deve ser criada uma instância da classe MyBroadcastReceiver(). Em seguida, deve-se definir quais mensagens ele deve receber, o que é feito através de um intent filter. Por fim, deve-se registrá-lo com uma chamada a registerReceiver(BroadcastReceiver, IntentFilter).</p>

```java
	BroadcastReceiver br = new MyBroadcastReceiver();
	IntentFilter filter = new IntentFilter(ConnectivityManager.CONNECTIVITY_ACTION);
    	filter.addAction(Intent.ACTION_BOOT_COMPLETED);
    	this.registerReceiver(br, filter);
```
<h6 align="center">Declaração de um broadcast receiver dinamicamente.</h6>

<br>
<h3>Intents</h3>
<p align="justify">Intents são mensagens enviadas entre os componentes de uma mesma aplicação, ou entre diferentes aplicações, para solicitar a funcionalidade deles. Intents são usados, basicamente, em três casos:</p>

<ul><li>Iniciar activities</li></ul>		
```java
	public class MainActivity extends AppCompatActivity {
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		
		Intent intent = new Intent(this, NewActivity.class);
		startActivity(intent);
	    }
	}
```
<h6 align="center">Envio de intent para iniciar uma activity.</h6>
		
```java
	public class NewActivity extends AppCompatActivity {
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
	
		Intent intent = getIntent();
	    }
	}
```
<h6 align="center">Recebimento de intent pela activity.</h6>
	
	
<ul><li>Iniciar services</li></ul>	
		
```java
	public class MainActivity extends AppCompatActivity {
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		
		Intent intent = new Intent(this, MyService.class);
		startService(intent);
	    }
	}
```
		
<h6 align="center">Envio de intent para iniciar um service.</h6>

```java
	public class MyService extends Service {
	    @Override
            public int onStartCommand(Intent intent, int flags, int startId) {
                return super.onStartCommand(intent, flags, startId);
            }
	}
```
<h6 align="center">Recebimento de intent pelo service.</h6>
	
<ul><li>Fazer um broadcast</li></ul>	

```java
	public class MainActivity extends AppCompatActivity {
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		
		Intent intent = new Intent();
		sendBroadcast(intent);
	    }
	}
```
		
<h6 align="center">Envio de intent para iniciar um broadcast.</h6>

```java
	public class MyBroadcastReceiver extends BroadcastReceiver {
            @Override
            public void onReceive(Context context, Intent intent) {
            	// aqui vai o código que diz o que fazer com o intent
            }
    	}
```
<h6 align="center">Recebimento de intent pelo broadcast receiver.</h6>

<p align="justify">Existem dois tipos de intents: explícitos e implícitos. Um intent explícito especifica qual aplicação ou componente irá recebê-lo. Já um intent implícito, ao invés de definir o componente que irá recebê-lo, define uma ação que deve ser realizada. Assim, qualquer aplicação capaz de realizar tal ação está apta a receber tal intent. Caso exista mais de uma possível aplicação, a escolha fica a cargo do usuário.</p>

<p align="justify">A definição de quais ações um componente pode realizar, ou seja, quais tipos de intents podem ser recebidos, é feita no manifesto através de intent filters.</p>

<br>
<h3>Intent filters</h3>
<p align="justify">Um intent filter especifica que tipo de intents são aceitos por um componente. Isso é feito através de três tags: <code class="language-plaintext highlighter-rouge">&lt;action&gt;</code>, <code class="language-plaintext highlighter-rouge">&lt;data&gt;</code> e <code class="language-plaintext highlighter-rouge">&lt;category&gt;</code>.</p>
	
```xml
	<activity android:name="ShareActivity">
	    <intent-filter>
		<action android:name="android.intent.action.SEND"/>
		<category android:name="android.intent.category.DEFAULT"/>
		<data android:mimeType="text/plain"/>
	    </intent-filter>
	</activity>
```
<h6 align="center">Declaração de um activity com um intent filter no AndroidManifest.</h6>
  
 
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
