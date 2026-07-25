import csv
import os

arquivo_usuarios='usuario.csv'

#vai inicia o banco de dados
def inicializacao_banco():
    """garante que o csv exista com as colunas corretas """
    if not os.path.exists(arquivo_usuarios):
        with open(arquivo_usuarios,'w',newline='',encoding='utf-8')as arq:
            escrito=csv.writer(arq)
            escrito.writerow(['usuario','senha'])

#ler o csv e ver todos os nomes salvo
def carregar_usuario():

    
    """ler o arquivo csv e devolver um dicionario com todos os usuarios e senhas"""
    usuario={}
    with open(arquivo_usuarios,'r',encoding='utf-8') as arq:
        leitor=csv.DictReader(arq)
        for linha in leitor:
            usuario[linha['usuario']] = linha['senha']
    return usuario

#cadastro de novo usuario no sistema que ainda nao existe
def cadastra_usuario():
    """caastra um novo usuario no sistema se ele ainda nao existe"""
    print('\n--TELA DE CADASTRO--')
    usuario=carregar_usuario()

    novo_usuario=input('qual e seu nome de usuario: ').strip().lower()

    if not novo_usuario:
        print("nome de usuario nao pode se vazio")
        return
    #verifica se o usuario esta cadastrado 
    if novo_usuario in usuario:
        print("esse usuario ja existe")
        return
    #CRIA SENHA    
    nova_senha=input("digite sua senha ").strip()

    if not nova_senha:
        print("senha nao pode esta vazia")
        return
    #salva no csv na ultimo
    with open(arquivo_usuarios,'a',newline='',encoding='utf-8') as arq:
        escrito = csv.writer(arq)
        escrito.writerow([novo_usuario,nova_senha])
    print(f"usuario '{novo_usuario}'cadastrado com sucesso")


#LOGIN
def fazer_login():
    """validar as credencias de login informadas pelo usuario"""
    print("\n ---TELA D LOGIN--")
    usuario = carregar_usuario()

    if not usuario:
        print("nem um usuario foi encontrado")
        return
    usuario_input=input("digite seu nome de usuario").strip().lower()
    senha_input=input("digite sua senha").strip()
    if usuario_input in usuario and usuario[usuario_input] == senha_input:
        print("login bem sucedido")
    else:
         print("\n usuario ou senha nao reconhecido")



def menu_principal():
    inicializacao_banco()

    while True:
        print("\n"+"="*30)
        print("SISTEMA DE ACESSO")
        print("="*30)
        print("1 cadastro de novo usuario")
        print("2 fazer login")
        print("3 sair do sistema")

        opcao = input("quale a opcao que voce quer(1,2,3)")
        if opcao == "1":
            cadastra_usuario()
        elif opcao == "2":
            fazer_login()
        elif opcao == "3":
            print("\n saindo do sistema")
            break
        else:
            print("opcao invalida")


menu_principal()

