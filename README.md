# Sistema_de_-hospedagem
public class Hospede
{
    public string Nome { get; set; }
    public string Sobrenome { get; set; }
    public DateTime DataNascimento { get; set; }
    public string Telefone { get; set; }
    public string Email { get; set; }
}


2. Crie uma classe chamada "Quarto" com as seguintes propriedades:


public class Quarto
{
    public int Numero { get; set; }
    public string Tipo { get; set; }
    public decimal PrecoPorNoite { get; set; }
    public bool Disponivel { get; set; }
}


3. Crie uma classe chamada "Reserva" com as seguintes propriedades:


public class Reserva
{
    public Hospede Hospede { get; set; }
    public Quarto Quarto { get; set; }
    public DateTime DataCheckIn { get; set; }
    public DateTime DataCheckOut { get; set; }
}


4. Crie uma classe chamada "Hotel" com as seguintes propriedades e métodos:


public class Hotel
{
    public List<Hospede> Hospedes { get; set; }
    public List<Quarto> Quartos { get; set; }
    public List<Reserva> Reservas { get; set; }

    public void AdicionarHospede(Hospede hospede)
    {
        Hospedes.Add(hospede);
    }

    public void AdicionarQuarto(Quarto quarto)
    {
        Quartos.Add(quarto);
    }

    public bool VerificarDisponibilidadeQuarto(int numeroQuarto, DateTime dataCheckIn, DateTime dataCheckOut)
    {
        var quarto = Quartos.FirstOrDefault(q => q.Numero == numeroQuarto && q.Disponivel);
        if (quarto == null)
        {
            return false;
        }

        var reserva = Reservas.FirstOrDefault(r =>
            r.Quarto.Numero == numeroQuarto &&
            ((dataCheckIn >= r.DataCheckIn && dataCheckIn < r.DataCheckOut) || (dataCheckOut > r.DataCheckIn && dataCheckOut <= r.DataCheckOut))
        );

        if (reserva != null)
        {
            return false;
        }

        return true;
    }

    public void AdicionarReserva(Reserva reserva)
    {
        var quarto = Quartos.FirstOrDefault(q => q.Numero == reserva.Quarto.Numero);
        if (quarto != null)
        {
            quarto.Disponivel = false;
            Reservas.Add(reserva);
        }
    }

    public List<Reserva> ListarReservas()
    {
        return Reservas;
    }    
}


5. Crie a interface gráfica do sistema usando o Windows Forms.

6. Implemente a lógica nos eventos dos botões da interface gráfica. Por exemplo, para adicionar um hóspede:


private void btnAdicionarHospede_Click(object sender, EventArgs e)
{
    var hospede = new Hospede
    {
        Nome = txtNome.Text,
        Sobrenome = txtSobrenome.Text,
        DataNascimento = dtpDataNascimento.Value,
        Telefone = txtTelefone.Text,
        Email = txtEmail.Text
    };

    hotel.AdicionarHospede(hospede);

    MessageBox.Show("Hóspede adicionado com sucesso!");
}
