import abc

class Cliente:
    def __init__(self, nome, cpf, endereco, telefone):
        self.nome = nome
        self.cpf = cpf
        self.endereco = endereco
        self.telefone = telefone
        self.contas = []

    def adicionar_conta(self, conta):
        self.contas.append(conta)

    def atualizar_dados(self, nome, endereco, telefone):
        if nome:
            self.nome = nome
        if endereco:
            self.endereco = endereco
        if telefone:
            self.telefone = telefone

class Conta(abc.ABC):
    def __init__(self, numero, cliente, saldo_inicial=0):
        self.numero = numero
        self.cliente = cliente
        self.saldo = saldo_inicial
        self.historico = []
        cliente.adicionar_conta(self)

    def depositar(self, valor):
        self.saldo += valor
        self.historico.append(f"Depósito: {valor}")

    def sacar(self, valor):
        if valor <= self.saldo:
            self.saldo -= valor
            self.historico.append(f"Saque: {valor}")
            return True
        return False

    def transferir(self, valor, conta_destino):
        if self.sacar(valor):
            conta_destino.depositar(valor)
            self.historico.append(f"Transferência enviada: {valor} para conta {conta_destino.numero}")
            conta_destino.historico.append(f"Transferência recebida: {valor} da conta {self.numero}")
            return True
        return False

    def pagar_boleto(self, valor, codigo_barras):
        if self.sacar(valor):
            self.historico.append(f"Pagamento de Boleto: {valor}, Código de Barras: {codigo_barras}")
            return True
        return False

    def exibir_historico(self):
        return self.historico

    def consultar_saldo(self):
        return self.saldo

class ContaCorrente(Conta):
    def __init__(self, numero, cliente, saldo_inicial=0):
        super().__init__(numero, cliente, saldo_inicial)

class ContaPoupanca(Conta):
    def __init__(self, numero, cliente, saldo_inicial=0):
        super().__init__(numero, cliente, saldo_inicial)

class Banco:
    def __init__(self):
        self.clientes = []
        self.contas = []

    def cadastrar_cliente(self, nome, cpf, endereco, telefone):
        cliente = Cliente(nome, cpf, endereco, telefone)
        self.clientes.append(cliente)
        return cliente

    def abrir_conta_corrente(self, cliente, saldo_inicial=0):
        numero_conta = self.gerar_numero_conta()
        conta = ContaCorrente(numero_conta, cliente, saldo_inicial)
        self.contas.append(conta)
        return conta

    def abrir_conta_poupanca(self, cliente, saldo_inicial=0):
        numero_conta = self.gerar_numero_conta()
        conta = ContaPoupanca(numero_conta, cliente, saldo_inicial)
        self.contas.append(conta)
        return conta

    def gerar_numero_conta(self):
        return len(self.contas) + 1

    def excluir_conta(self, conta):
        if conta.saldo == 0:
            self.contas.remove(conta)
            conta.cliente.contas.remove(conta)
            return True
        return False


banco = Banco()

cliente1 = banco.cadastrar_cliente("João Silva", "12345678900", "Rua A, 123", "11999999999")
print(f"Cliente cadastrado: {cliente1.nome}, CPF: {cliente1.cpf}, Endereço: {cliente1.endereco}, Telefone: {cliente1.telefone}")

cliente2 = banco.cadastrar_cliente("Maria Santos", "09876543211", "Rua B, 456", "11988888888")
print(f"Cliente cadastrado: {cliente2.nome}, CPF: {cliente2.cpf}, Endereço: {cliente2.endereco}, Telefone: {cliente2.telefone}")

conta_corrente = banco.abrir_conta_corrente(cliente1, 1000)
print(f"Conta corrente criada para {cliente1.nome}, Número da conta: {conta_corrente.numero}, Saldo inicial: {conta_corrente.saldo}")

conta_poupanca = banco.abrir_conta_poupanca(cliente2, 200)
print(f"Conta poupança criada para {cliente2.nome}, Número da conta: {conta_poupanca.numero}, Saldo inicial: {conta_poupanca.saldo}")

conta_corrente.depositar(500)
print(f"Depósito realizado na conta {conta_corrente.numero}. Novo saldo: {conta_corrente.saldo}")

conta_corrente.transferir(300, conta_poupanca)
print(f"Transferência realizada da conta {conta_corrente.numero} para a conta {conta_poupanca.numero}. Saldo da conta corrente: {conta_corrente.saldo}. Saldo da conta poupança: {conta_poupanca.saldo}")

print(f"Saldo da conta corrente {conta_corrente.numero}: {conta_corrente.consultar_saldo()}")
print(f"Histórico da conta corrente {conta_corrente.numero}: {conta_corrente.exibir_historico()}")

conta_corrente.pagar_boleto(100, "12345.67890 12345.678901 12345.678901 1 23456789012345")
print(f"Pagamento de boleto realizado na conta {conta_corrente.numero}. Novo saldo: {conta_corrente.saldo}")
print(f"Histórico da conta corrente {conta_corrente.numero}: {conta_corrente.exibir_historico()}")

print(f"Saldo da conta poupança {conta_poupanca.numero}: {conta_poupanca.consultar_saldo()}")
print(f"Histórico da conta poupança {conta_poupanca.numero}: {conta_poupanca.exibir_historico()}")
