# Mail-g-nderme
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Data.Sql;
using System.Data.SqlClient;
using System.Net;
using System.Net.Mail;

namespace sifre_unuttum_mail_gonderme
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void label2_Click(object sender, EventArgs e)
        {

        }

        private void label3_Click(object sender, EventArgs e)
        {

        }

        private void button1_Click(object sender, EventArgs e)
        {
            sqlbaglantisi bgln = new sqlbaglantisi();
            SqlConnection baglanti=new SqlConnection("Data Source=LENOVO\\SQLEXPRESS;Initial Catalog=proje;Integrated Security=True");

            SqlDataReader oku =baglanti.ExecuteReader;
            while (oku.Read())
            {
                try
                {
                    if (bgln.baglanti().State == ConnectionState.Closed)
                    {
                        bgln.baglanti().Open();
                    }
                    SmtpClient smtpserver = new SmtpClient();
                    MailMessage mail = new MailMessage();
                    string tarih = DateTime.Now.ToLongDateString();
                    string mailadresin = ("firatyazilimdersi@gmail.com");
                    string sifre = ("firatyazilimdersi123");
                    string smptsrvr = "smtp.gmail.com";
                    string kime = (oku["eposta"].ToString());
                    string konu = ("Şifre Hatırlatma Maili");
                    string yaz = ("Sayın," + oku["ad_soyad"].ToString() + "\n" + "bizden " + tarih + " tarihinde şifre hatırlatma mesajı bulundu" + "\n" + "parolanız" + oku["şifre"].ToString() + "\niyi günler");
                    smtpserver.Credentials = new NetworkCredential(mailadresin, sifre);
                    smtpserver.Port = 587;
                    smtpserver.Host = smptsrvr;
                    smtpserver.EnableSsl= true;
                    mail.From= new MailAddress(mailadresin);
                    mail.To.Add(kime);
                    mail.Subject = konu;
                    mail.Body = yaz;
                    smtpserver.Send(mail);
                    DialogResult bilgi= new DialogResult();
                    bilgi = MessageBox.Show("Girmiş olduğunuz bligiler uyuşuyor. şifreniz mail adresinize gönderilmiştir");
                    this.Hide();

                }
                catch(Exception Hata)
                {
                    MessageBox.Show("mail gönderme hatası!", Hata.Message);
                }
            }
        }
    }
}
