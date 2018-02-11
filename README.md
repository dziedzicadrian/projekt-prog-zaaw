<h2> Program do pingowania adresów internetowych. </h2>
<h0> Program pinguje strony internetowe i wyświetla adresy IP pod jakimi się połączyły</h0>
<h0> Poniżej jest kod źródłowy</h0>
using System;<br />
using System.Collections.Generic;<br />
using System.ComponentModel;<br />
using System.Data;<br />
using System.Drawing;<br />
using System.Linq;<br />
using System.Text;<br />
using System.Threading.Tasks;<br />
using System.Windows.Forms;<br />
using System.Net.NetworkInformation;<br /><br />

namespace Ping_Check
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button2_Click(object sender, EventArgs e)
        {
            if (textBox2.Text != String.Empty)
                if (textBox2.Text.Trim().Length>0)
                {
                    listBox2.Items.Add(textBox2.Text);
                    textBox1.Clear();
                }
        }

        private void listBox2_DoubleClick(object sender, EventArgs e)
        {
            if (listBox2.SelectedIndex != -1)
                listBox2.Items.RemoveAt(listBox2.SelectedIndex);
        }

        private string WyślijPing(string adres, int timeout, byte[] bufor, PingOptions opcje)
        {
            Ping ping = new Ping();
            try
            {
                PingReply odpowiedz = ping.Send(adres, timeout, bufor, opcje);
                if (odpowiedz.Status == IPStatus.Success)
                    return "Odpowiedź z " + adres + " bajtów= " + odpowiedz.Buffer.Length + " czas= " + odpowiedz.RoundtripTime + " ms TTL= " + odpowiedz.Options.Ttl;
                else
                    return "Błąd :" + adres + " " + odpowiedz.Status.ToString();

            }
            catch (Exception ex)
            {
                return "Błąd :" + adres + " " + ex.Message;
            }
        }

        private void button1_Click(object sender, EventArgs e)
        {
            if (textBox1.Text != "" || listBox2.Items.Count >0)
            {
                PingOptions opcje = new PingOptions();
                opcje.Ttl = (int)numericUpDown2.Value;
                opcje.DontFragment = true;
                string dane = "bbbbbbbbbbbbbbbb";
                byte[] bufor = Encoding.ASCII.GetBytes(dane);
                int timeout = 120;
                if (textBox1.Text != "")
                {
                    for (int i = 0; i < (int)numericUpDown1.Value; i++)
                        listBox1.Items.Add(this.WyślijPing(textBox1.Text, timeout, bufor, opcje));
                    listBox1.Items.Add("_________________");
                }
                if (listBox2.Items.Count>0)
                {
                    foreach (string host in listBox2.Items)
                    {
                        for (int i = 0;i<(int)numericUpDown1.Value; i++)
                            listBox1.Items.Add(this.WyślijPing(host, timeout, bufor, opcje));
                        listBox1.Items.Add("_______________________");
                    }
                }
            }
            else
            {
                MessageBox.Show("Nie wprowadziłeś żadnego adresu " + "Błąd");
            }
        }
    }
}
