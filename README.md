DESCRIÇÃO
Neste desafio pensando na otimização do nosso código iremos separar as funções existentes de saque, depósito e extrato em funções: cadastrar usuário (cliente) e cadastrar conta bancária.

# desafio python (separar função)

def depositar(saldo, extrato):
    valor = float(input("Informe o valor do depósito: "))
    if valor > 0:
        saldo += valor
        extrato += f"Depósito: R$ {valor:.2f}\n"
    else:
        print("Operação falhou! O valor informado é inválido.")
    return saldo, extrato


def sacar(saldo, extrato, numero_saques, limite_saques, limite):
    valor = float(input("Informe o valor do saque: "))
    excedeu_saldo = valor > saldo
    excedeu_limite = valor > limite
    excedeu_saques = numero_saques >= limite_saques
    if excedeu_saldo:
        print("Operação falhou! Você não tem saldo suficiente.")
    elif excedeu_limite:
        print("Operação falhou! O valor do saque excede o limite.")
    elif excedeu_saques:
        print("Operação falhou! Número máximo de saques excedido.")
    elif valor > 0:
        saldo -= valor
        extrato += f"Saque: R$ {valor:.2f}\n"
        numero_saques += 1
    else:
        print("Operação falhou! O valor informado é inválido.")
    return saldo, extrato, numero_saques


def extrato_bancario(saldo, extrato):
    print("\n================ EXTRATO ================")
    print("Não foram realizadas movimentações." if not extrato else extrato)
    print(f"\nSaldo: R$ {saldo:.2f}")
    print("==========================================")


def main():
    saldo = 0
    limite = 500
    extrato = ""
    numero_saques = 0
    limite_saques = 3

    while True:
        opcao = input("""
[d] Depositar
[s] Sacar
[e] Extrato
[q] Sair
=> """)

        if opcao == "d":
            saldo, extrato = depositar(saldo, extrato)

        elif opcao == "s":
            saldo, extrato, numero_saques = sacar(saldo, extrato, numero_saques, limite_saques, limite)

        elif opcao == "e":
            extrato_bancario(saldo, extrato)

        elif opcao == "q":
            break

        else:
            print("Operação inválida, por favor selecione novamente a operação desejada.")


if __name__ == "__main__":
    main()

# vincular conta 

clientes = []
contas = []

class Cliente:
    def __init__(self, nome, cpf):
        self.nome = nome
        self.cpf = cpf

class Conta:
    def __init__(self, numero, cliente, saldo=0, limite=500):
        self.numero = numero
        self.cliente = cliente
        self.saldo = saldo
        self.limite = limite
        self.extrato = ""
        self.numero_saques = 0
        self.limite_saques = 3

def cadastrar_cliente():
    nome = input("Digite o nome do cliente: ")
    cpf = input("Digite o CPF do cliente: ")
    cliente = Cliente(nome, cpf)
    clientes.append(cliente)
    return cliente

def cadastrar_conta():
    numero = input("Digite o número da conta: ")
    cliente_cpf = input("Digite o CPF do cliente: ")
    cliente = None
    for c in clientes:
        if c.cpf == cliente_cpf:
            cliente = c
            break
    if cliente is None:
        print("Cliente não encontrado.")
        return None
    conta = Conta(numero, cliente)
    contas.append(conta)
    return conta

def depositar(conta):
    valor = float(input("Informe o valor do depósito: "))
    if valor > 0:
        conta.saldo += valor
        conta.extrato += f"Depósito: R$ {valor:.2f}\n"
    else:
        print("Operação falhou! O valor informado é inválido.")

def sacar(conta):
    valor = float(input("Informe o valor do saque: "))
    excedeu_saldo = valor > conta.saldo
    excedeu_limite = valor > conta.limite
    excedeu_saques = conta.numero_saques >= conta.limite_saques
    if excedeu_saldo:
        print("Operação falhou! Você não tem saldo suficiente.")
    elif excedeu_limite:
        print("Operação falhou! O valor do saque excede o limite.")
    elif excedeu_saques:
        print("Operação falhou! Número máximo de saques excedido.")
    elif valor > 0:
        conta.saldo -= valor
        conta.extrato += f"Saque: R$ {valor:.2f}\n"
        conta.numero_saques += 1
    else:
        print("Operação falhou! O valor informado é inválido.")

def extrato(conta):
    print("\n================ EXTRATO ================")
    print("Não foram realizadas movimentações." if not conta.extrato else conta.extrato)
    print(f"\nSaldo: R$ {conta.saldo:.2f}")
    print("==========================================")

def main():
    while True:
        opcao = input("""
[c] Cadastrar cliente
[t] Cadastrar conta
[d] Depositar
[s] Sacar
[e] Extrato
[q] Sair
=> """)

        if opcao == "c":
            cadastrar_cliente()

        elif opcao == "t":
            cadastrar_conta()

        elif opcao == "d":
            numero_conta = input("Digite o número da conta: ")
            conta = None
            for c in contas:
                if c.numero == numero_conta:
                    conta = c
                    break
            if conta is None:
                print("Conta não encontrada.")
            else:
                depositar(conta)

        elif opcao == "s":
            numero_conta = input("Digite o número da conta: ")
            conta = None
            for c in contas:
                if c.numero == numero_conta:
                    conta = c
                    break
            if conta is None:
                print("Conta não encontrada.")
            else:
                sacar(conta)

        elif opcao == "e":
            numero_conta = input("Digite o número da conta: ")
            conta = None
            for c in contas:
                if c.numero == numero_conta:
                    conta = c
                    break
            if conta is None:
                print("Conta não encontrada.")
            else:
                extrato(conta)

        elif opcao == "q":
            break

        else:
            print("Operação inválida, por favor selecione novamente a operação desejada.")

if __name__ == "__main__":
    main()

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

       
        ### exemplo de conclução (vinculando conta)

clientes = []
contas = []

class Cliente:
    def __init__(self, nome, cpf):
        self.nome = nome
        self.cpf = cpf
        self.contas = []

class Conta:
    def __init__(self, numero, cliente, saldo=0, limite=500):
        self.numero = numero
        self.cliente = cliente
        self.saldo = saldo
        self.limite = limite
        self.extrato = ""
        self.numero_saques = 0
        self.limite_saques = 3

def cadastrar_cliente():
    nome = input("Digite o nome do cliente: ")
    cpf = input("Digite o CPF do cliente: ")
    cliente = Cliente(nome, cpf)
    clientes.append(cliente)
    return cliente

def cadastrar_conta():
    numero = input("Digite o número da conta: ")
    cliente_cpf = input("Digite o CPF do cliente: ")
    cliente = None
    for c in clientes:
        if c.cpf == cliente_cpf:
            cliente = c
            break
    if cliente is None:
        print("Cliente não encontrado.")
        return None
    conta = Conta(numero, cliente)
    cliente.contas.append(conta)
    contas.append(conta)
    return conta

def depositar(numero_conta):
    conta = None
    for c in contas:
        if c.numero == numero_conta:
            conta = c
            break
    if conta is None:
        print("Conta não encontrada.")
        return
    valor = float(input("Informe o valor do depósito: "))
    if valor > 0:
        conta.saldo += valor
        conta.extrato += f"Depósito: R$ {valor:.2f}\n"
    else:
        print("Operação falhou! O valor informado é inválido.")

def sacar(numero_conta):
    conta = None
    for c in contas:
        if c.numero == numero_conta:
            conta = c
            break
    if conta is None:
        print("Conta não encontrada.")
        return
    valor = float(input("Informe o valor do saque: "))
    excedeu_saldo = valor > conta.saldo
    excedeu_limite = valor > conta.limite
    excedeu_saques = conta.numero_saques >= conta.limite_saques
    if excedeu_saldo:
        print("Operação falhou! Você não tem saldo suficiente.")
    elif excedeu_limite:
        print("Operação falhou! O valor do saque excede o limite.")
    elif excedeu_saques:
        print("Operação falhou! Número máximo de saques excedido.")
    elif valor > 0:
        conta.saldo -= valor
        conta.extrato += f"Saque: R$ {valor:.2f}\n"
        conta.numero_saques += 1
    else:
        print("Operação falhou! O valor informado é inválido.")

def extrato(numero_conta):
    conta = None
    for c in contas:
        if c.numero == numero_conta:
            conta = c
            break
    if conta is None:
        print("Conta não encontrada.")
        return
    print("\n================ EXTRATO ================")
    print("Não foram realizadas movimentações." if not conta.extrato else conta.extrato)
    print(f"\nSaldo: R$ {conta.saldo:.2f}")
    print("==========================================")

def main():
    while True:
        opcao = input("""
[c] Cadastrar cliente
[t] Cadastrar conta
[d] Depositar
[s] Sacar
[e] Extrato
[q] Sair
=> """)

        if opcao == "c":
            cadastrar_cliente()

        elif opcao == "t":
            cadastrar_conta()

        elif opcao == "d":
            numero_conta = input("Digite o número da conta: ")
            depositar(numero_conta)

        elif opcao == "s":
            numero_conta = input("Digite o número da conta:Para vincular uma conta corrente com um cliente, você pode adicionar um atributo `contas` na classe `Cliente`, que armazena uma lista de contas bancárias associadas a esse cliente. Além disso, no momento de criar uma nova conta bancária, você pode solicitar o CPF do cliente e buscar o objeto `Cliente` correspondente na lista `clientes`. Em seguida, você pode criar a nova conta e adicioná-la à lista de contas do cliente.

Aqui está um exemplo de como você pode alterar as funções `cadastrar_cliente` e `cadastrar_conta` para vincular uma conta corrente com um cliente:

```python
clientes = []
contas = []

class Cliente:
    def __init__(self, nome, cpf):
        self.nome = nome
        self.cpf = cpf
        self.contas = []

class Conta:
    def __init__(self, numero, cliente, saldo=0, limite=500):
        self.numero = numero
        self.cliente = cliente
        self.saldo = saldo
        self.limite = limite
        self.extrato = ""
        self.numero_saques = 0
        self.limite_saques = 3

def cadastrar_cliente():
    nome = input("Digite o nome do cliente: ")
    cpf = input("Digite o CPF do cliente: ")
    cliente = Cliente(nome, cpf)
    clientes.append(cliente)
    return cliente

def cadastrar_conta():
    numero = input("Digite o número da conta: ")
    cliente_cpf = input("Digite o CPF do cliente: ")
    cliente = None
    for c in clientes:
        if c.cpf == cliente_cpf:
            cliente = c
            break
    if cliente is None:
        print("Cliente não encontrado.")
        return None
    conta = Conta(numero, cliente)
    cliente.contas.append(conta)
    contas.append(conta)
    return conta

def depositar(numero_conta):
    conta = None
    for c in contas:
        if c.numero == numero_conta:
            conta = c
            break
    if conta is None:
        print("Conta não encontrada.")
        return
    valor = float(input("Informe o valor do depósito: "))
    if valor > 0:
        conta.saldo += valor
        conta.extrato += f"Depósito: R$ {valor:.2f}\n"
    else:
        print("Operação falhou! O valor informado é inválido.")

def sacar(numero_conta):
    conta = None
    for c in contas:
        if c.numero == numero_conta:
            conta = c
            break
    if conta is None:
        print("Conta não encontrada.")
        return
    valor = float(input("Informe o valor do saque: "))
    excedeu_saldo = valor > conta.saldo
    excedeu_limite = valor > conta.limite
    excedeu_saques = conta.numero_saques >= conta.limite_saques
    if excedeu_saldo:
        print("Operação falhou! Você não tem saldo suficiente.")
    elif excedeu_limite:
        print("Operação falhou! O valor do saque excede o limite.")
    elif excedeu_saques:
        print("Operação falhou! Número máximo de saques excedido.")
    elif valor > 0:
        conta.saldo -= valor
        conta.extrato += f"Saque: R$ {valor:.2f}\n"
        conta.numero_saques += 1
    else:
        print("Operação falhou! O valor informado é inválido.")

def extrato(numero_conta):
    conta = None
    for c in contas:
        if c.numero == numero_conta:
            conta = c
            break
    if conta is None:
        print("Conta não encontrada.")
        return
    print("\n================ EXTRATO ================")
    print("Não foram realizadas movimentações." if not conta.extrato else conta.extrato)
    print(f"\nSaldo: R$ {conta.saldo:.2f}")
    print("==========================================")

def main():
    while True:
        opcao = input("""
[c] Cadastrar cliente
[t] Cadastrar conta
[d] Depositar
[s] Sacar
[e] Extrato
[q] Sair
=> """)

        if opcao == "c":
            cadastrar_cliente()

        elif opcao == "t":
            cadastrar_conta()

        elif opcao == "d":
            numero_conta = input("Digite o número da conta: ")
            depositar(numero_conta)

        elif opcao == "s":
            numero_conta = input("Digite o número da conta:
        
