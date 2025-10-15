# 📱 AppLogin – Aplicação de Login e Cadastro em Android

Um projeto Android simples para cadastrar usuários e autenticar login usando banco de dados local **SQLite**.

---

## 🚀 Sobre o Projeto

O **AppLogin** é um aplicativo que permite cadastrar um usuário com login e senha, armazenando esses dados no banco SQLite, para que ele possa autenticar-se posteriormente.

O app possui três telas principais:

- **Tela 1 – Início:** Tela principal com botões para entrar ou registrar novo usuário.
- **Tela 2 – Cadastro:** Formulário para cadastrar novo usuário com validação das senhas.
- **Tela 3 – Autenticação:** Formulário para login do usuário já cadastrado.

---

## 🛠️ Tecnologias e Ferramentas

| Categoria | Ferramenta                   |
|-----------|-----------------------------|
| IDE       | Android Studio              |
| Linguagem | Java                        |
| Banco     | SQLite (via SQLiteOpenHelper) |
| Layout    | XML                         |

---

## 📁 Estrutura do Projeto

app/
├── java/
│ └── br/com/senac/appLogin/
│ ├── MainActivity.java
│ ├── DBHelper.java
│ ├── LoginActivity.java
│ └── RegistrarActivity.java
├── res/
│ ├── layout/
│ │ ├── activity_main.xml
│ │ ├── activity_login.xml
│ │ └── activity_registrar.xml
└── AndroidManifest.xml

csharp
Copiar código

---

## 📋 Descrição das Classes

### MainActivity.java
Tela inicial que exibe botões para acessar a tela de login ou cadastro.

```java
public class MainActivity extends AppCompatActivity {
    Button btEntrar, btRegistar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btEntrar = findViewById(R.id.btLogin);
        btRegistar = findViewById(R.id.btRegistrar);

        btEntrar.setOnClickListener(view -> {
            Intent i = new Intent(MainActivity.this, LoginActivity.class);
            startActivity(i);
        });

        btRegistar.setOnClickListener(view -> {
            Intent i = new Intent(MainActivity.this, RegistrarActivity.class);
            startActivity(i);
        });
    }

}

## DBHelper.java
Classe que gerencia o banco de dados SQLite: criação da tabela, inserção de usuários e validação do login.

java
Copiar código
public class DBHelper extends SQLiteOpenHelper {
    private static String nome = "BancoDados.db";
    private static int versao = 1;

    public DBHelper(Context context) {
        super(context, nome, null, versao);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        String str = "CREATE TABLE utilizador(username TEXT PRIMARY KEY, password TEXT);";
        db.execSQL(str);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS utilizador;");
        onCreate(db);
    }

    public long criarUtilizador(String userName, String password) {
        SQLiteDatabase db = getWritableDatabase();
        ContentValues cv = new ContentValues();
        cv.put("username", userName);
        cv.put("password", password);
        return db.insert("utilizador", null, cv);
    }

    public String validarLogin(String userName, String password) {
        SQLiteDatabase db = getReadableDatabase();
        Cursor c = db.rawQuery("SELECT * FROM utilizador WHERE username=? AND password=?", new String[]{userName, password});
        if (c.getCount() > 0) {
            return "OK";
        }
        return "ERRO";
    }
}
## RegistrarActivity.java
Tela para cadastro de novos usuários, valida as senhas e salva no banco.

java
Copiar código
public class RegistrarActivity extends AppCompatActivity {
    EditText edNome, edUser, edPas1, edPas2;
    Button btSalvar;
    DBHelper db;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_registrar);

        db = new DBHelper(this);

        edNome = findViewById(R.id.edNome);
        edUser = findViewById(R.id.edUser);
        edPas1 = findViewById(R.id.edPass1);
