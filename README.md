import java.time.LocalDate;

// Classe abstrata Conta (superclasse)
abstract class Conta {
    protected double saldo;
    protected String numero;

    public Conta(double saldoInicial, String numero) {
        this.saldo = saldoInicial;
        this.numero = numero;
    }

    public double getSaldo() {
        return saldo;
    }

    public String getNumero() {
        return numero;
    }

    public abstract void depositar(double valor);

    public abstract void sacar(double valor);
}

// Conta Corrente
class ContaCorrente extends Conta {
    private double chequeEspecial;

    public ContaCorrente(double saldoInicial, String numero, double chequeEspecial) {
        super(saldoInicial, numero);
        this.chequeEspecial = chequeEspecial;
    }

    @Override
    public void depositar(double valor) {
        saldo += valor;
    }

    @Override
    public void sacar(double valor) {
        double limiteDisponivel = saldo + chequeEspecial;
        if (valor <= limiteDisponivel) {
            saldo -= valor;
        } else {
            System.out.println("Saldo insuficiente. Limite disponível para saque: R$" + limiteDisponivel);
        }
    }

    public double getChequeEspecial() {
        return chequeEspecial;
    }
}

// Conta Poupança
class ContaPoupanca extends Conta {
    private static final double SALDO_MINIMO = 2000.0;
    private static final double RENDIMENTO_ANUAL = 0.006;
    private LocalDate aniversarioConta;

    public ContaPoupanca(double saldoInicial, String numero, LocalDate aniversarioConta) {
        super(saldoInicial, numero);
        this.aniversarioConta = aniversarioConta;
    }

    @Override
    public void depositar(double valor) {
        saldo += valor;
    }

    @Override
    public void sacar(double valor) {
        if (saldo - valor >= SALDO_MINIMO) {
            saldo -= valor;
        } else {
            System.out.println("Saldo insuficiente para saque.");
        }
    }

    public void renderJuros() {
        saldo += saldo * RENDIMENTO_ANUAL;
    }

    public void aplicarTaxaDeJuros(LocalDate hoje) {
        if (hoje.equals(aniversarioConta)) {
            renderJuros();
            System.out.println("Juros aplicados! Saldo atualizado: R$" + saldo);
        } else {
            System.out.println("Hoje não é o dia de aniversário da conta. Juros não aplicados.");
        }
    }

    public LocalDate getAniversarioConta() {
        return aniversarioConta;
    }
}

// Conta Salário
class ContaSalario extends Conta {
    private int limiteSaques;
    private static final int MAX_SAQUES = 2;

    public ContaSalario(double saldoInicial, String numero) {
        super(saldoInicial, numero);
        this.limiteSaques = MAX_SAQUES;
    }

    @Override
    public void depositar(double valor) {
        saldo += valor;
    }

    @Override
    public void sacar(double valor) {
        if (limiteSaques > 0) {
            saldo -= valor;
            limiteSaques--;
        } else {
            System.out.println("Limite de saques atingido.");
        }
    }

    public int getLimiteSaques() {
        return limiteSaques;
    }
}
