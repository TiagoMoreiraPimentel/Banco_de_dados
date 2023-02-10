# Validação de acesso com login e senha:

## codigo de conexao_DAO

    package App.DAO;

    import java.sql.Connection;
    import javax.swing.JOptionPane;
    import java.sql.DriverManager;
    import java.sql.SQLException;

    // classe responsavel por realizar a conexao com os objetos do banco de dados
    public class ConexaoDAO {

        // metodo que conecta com o banco de dados
        public Connection ConectaBD() {

            Connection conn = null;

            try {

                String url = "jdbc:mysql://localhost:3306/validacao?user=root&password=153351153351";

                conn = DriverManager.getConnection(url);

            } catch (SQLException erro) {
                JOptionPane.showMessageDialog(null, "ConexaoDAO: " + erro.getMessage());
            }

            return conn;
        }
    }

## codigo que consulta informações no banco de dados comparando

    package App.DAO;

    import App.entities.ValidarLogin;
    import java.sql.PreparedStatement;
    import java.sql.Connection;
    import java.sql.ResultSet;
    import java.sql.SQLException;
    import javax.swing.JOptionPane;

    // clase que consulta as informações no banco de dados
    public class UsuarioDAO {

        Connection conn;

        public ResultSet autenticacaoUsuario(ValidarLogin objvalidarlogin) {

            conn = new ConexaoDAO().ConectaBD();

            try {
                String sql = "select * from usuario where nome_usuario = ? and senha_usuario = ? ";

                PreparedStatement pstm = conn.prepareStatement(sql);
                pstm.setString(1, objvalidarlogin.getLogin());
                pstm.setString(2, objvalidarlogin.getSenha());

                ResultSet rs = pstm.executeQuery();

                return rs;

            } catch (SQLException erro) {
                JOptionPane.showMessageDialog(null, "UsuarioDAO: " + erro.getMessage());
                return null;
            }

        }

    }

## Codigo que vai dentro do VIEW para acionar a autenticação do banco de dados validando login e senha

	try {
            
            String field_login = jTextField_login.getText();
            String password_senha = jPasswordField_senha.getText();

            ValidarLogin validarlogin = new ValidarLogin();

            validarlogin.setLogin(field_login);
            validarlogin.setSenha(password_senha);
            
            UsuarioDAO objusuariodao = new UsuarioDAO();
            ResultSet rsusuariodao = objusuariodao.autenticacaoUsuario(validarlogin);
            
            if (rsusuariodao.next()) {
                JOptionPane.showMessageDialog(null, "Login efetuado, Bem vindo!");
            }else{
                JOptionPane.showMessageDialog(null, "Login ou senha incorreta!");
            }    
            
        }catch (SQLException erro){
            JOptionPane.showMessageDialog(null, "TelaLogin: " + erro.getMessage());
        }

# Cadastro de clientes

## codigo de conexao_DAO

    package App.DAO;

    import java.sql.Connection;
    import javax.swing.JOptionPane;
    import java.sql.DriverManager;
    import java.sql.SQLException;

    public class ConexaoCadastroDAO {

        // metodo que conecta com o banco de dados
        public Connection ConectacadastroBD() {

            Connection conn = null;

            try {

                String url = "jdbc:mysql://localhost:3306/cadastroteste?user=root&password=153351153351";

                conn = DriverManager.getConnection(url);

            } catch (SQLException erro) {
                JOptionPane.showMessageDialog(null, "ConexaoDAO: " + erro.getMessage());
            }

            return conn;
        }

    }

## codigo que consulta e realiza o cadastro dos dados no banco

    package App.DAO;

    import App.entities.CadastrodeClientes;
    import java.sql.PreparedStatement;
    import java.sql.Connection;
    import java.sql.SQLException;
    import javax.swing.JOptionPane;

    public class CadastroClientesDAO {
        
        Connection conn;

        public void cadastrarClientes(CadastrodeClientes objcadastrodeclientes) {

            conn = new ConexaoCadastroDAO().ConectacadastroBD();

            try {
        
                String sql = "insert into cadastroclientes (nome_cliente, endereco_cliente, email_cliente) values (?, ?, ?)";

                PreparedStatement pstm = conn.prepareStatement(sql);
                pstm.setString(1, objcadastrodeclientes.getNome());
                pstm.setString(2, objcadastrodeclientes.getEndereco());
                pstm.setString(3, objcadastrodeclientes.getEmail());

                pstm.execute();
                pstm.close();

            } catch (SQLException erro) {
                JOptionPane.showMessageDialog(null, "ConexaoCadastroDAO: " + erro.getMessage());

            }

        }
        
    }

## Codigo que vai dentro do VIEW para acionar o cadastro

    String nomeclientes = jTextField_nome.getText();
    String enderecoclientes = jTextField_endereco.getText();
    String emailclientes = jTextField_email.getText();

    CadastrodeClientes cadastrodeclientes = new CadastrodeClientes();

    cadastrodeclientes.setNome(nomeclientes);
    cadastrodeclientes.setEndereco(enderecoclientes);
    cadastrodeclientes.setEmail(emailclientes);

    CadastroClientesDAO cadastroclientesdao = new CadastroClientesDAO();
    cadastroclientesdao.cadastrarClientes(cadastrodeclientes);

    JOptionPane.showMessageDialog(null, "Cadastro realizado com sucesso!");