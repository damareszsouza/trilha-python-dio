# Trilha Python DIO
clientes = {}
contas = {}

def cadastrar_cliente(nome, cpf):
    clientes[cpf] = nome
    print(f"Cliente {nome} cadastrado com sucesso!")

def criar_conta(cpf):
    if cpf not in clientes:
        print("Cliente não encontrado.")
    elif cpf in contas:
        print("O cliente já possui uma conta bancária.")
    else:
        num_conta = len(contas) + 1
        contas[cpf] = {"numero": num_conta, "saldo": 0}
        print(f"Conta bancária número {num_conta} criada com sucesso para o cliente {clientes[cpf]}.")

def depositar(cpf, valor):
    if cpf not in clientes:
        print("Cliente não encontrado.")
    elif cpf not in contas:
        print("O cliente não possui uma conta bancária.")
    else:
        contas[cpf]["saldo"] += valor
        print(f"Depósito de R${valor:.2f} realizado com sucesso na conta bancária número {contas[cpf]['numero']}.")

def sacar(cpf, valor):
    if cpf not in clientes:
        print("Cliente não encontrado.")
    elif cpf not in contas:
        print("O cliente não possui uma conta bancária.")
    elif contas[cpf]["saldo"] < valor:
        print("Saldo insuficiente.")
    else:
        contas[cpf]["saldo"] -= valor
        print(f"Saque de R${valor:.2f} realizado com sucesso na conta bancária número {contas[cpf]['numero']}.")

def consultar_saldo(cpf):
    if cpf not in clientes:
        print("Cliente não encontrado.")
    elif cpf not in contas:
        print("O cliente não possui uma conta bancária.")
    else:
        print(f"Saldo atual da conta bancária número {contas[cpf]['numero']}: R${contas[cpf]['saldo']:.2f}.")
