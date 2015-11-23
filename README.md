using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
        
        private void btnExit_Click(object sender, EventArgs e)
        {
            Close();
        }
        
        private void scrb_Scroll(object sender, ScrollEventArgs e)
        {
            lblB.Text = String.Concat("b = ", (scrb.Value).ToString());
        }

        private void pctDraw_Click(object sender, EventArgs e)
        {

        }
        static int a = 35;
        private void func(float f, int b, out float x, out float y)
        {
            x = (float)((a * (Math.Cos(f) + Math.Sqrt(3 + Math.Pow(Math.Cos(f), 2) )* Math.Pow(Math.Cos(3.5 * f), 2) - b * Math.Sin(30 * f))) * Math.Cos(f));
            y = (float)((a * (Math.Cos(f) + Math.Sqrt(3 + Math.Pow(Math.Cos(f), 2)) * Math.Pow(Math.Cos(3.5 * f), 2) - b * Math.Sin(30 * f))) * Math.Sin(f));
        }

        
        private void btnDraw_Click(object sender, EventArgs e)
        {
            int b = scrb.Value;
            float h = 0.01f, k_max = 0, tt, x1, y1, x2, y2; // Определение масштаба          
            for (float f = 0; f <= 2 * Math.PI; f = f + h)
            {
                func(f, b, out x2, out y2);
                if (f == 0) k_max = (x2 > y2) ? x2 : y2;
                else
                {
                    tt = (x2 > y2) ? x2 : y2;
                    k_max = (tt > k_max) ? tt : k_max; ;
                }
            } 
            int pict_size = (pctDraw.Width < pctDraw.Height) ? pctDraw.Width : pctDraw.Height;

            pict_size /= 2; 
            // Подготовка к рисованию             
            Graphics g =  pctDraw.CreateGraphics();
            g.Clear(btnColor.BackColor);                
            g.ScaleTransform(pict_size/(float)k_max, pict_size/(float)k_max);              
            g.TranslateTransform(pctDraw.Width/2, pctDraw.Height/2, System.Drawing.Drawing2D.MatrixOrder.Append);             
            Pen p = new Pen(Color.Black, hcrLine.Value);              
            // Оси координат             
            g.DrawLine(p, -pctDraw.Width/2, 0, pctDraw.Width/2, 0);              
            g.DrawLine(p, 0, -pctDraw.Height/2, 0, pctDraw.Height/2);             
            // Рисование графика функции             
            p.Color = btnColorLine.BackColor;              
            func(0, b, out x1, out y1);              
            float xmax=0, ymax=0;             
            for(float f=h;f<=2*Math.PI+h;f=f+h)     
            {     
                func(f, b, out x2, out y2);      
                g.DrawLine(p, x1, y1, x2, y2);                  
                x1 = x2;                  
                y1 = y2;                  
                xmax=(x1>xmax) ? x1 : xmax;                  
                ymax=(y1>ymax) ? y1 : ymax;              
            }             
            // Подпись осей координат             
            g.DrawString( (xmax).ToString(),new System.Drawing.Font("Arial", 8), new SolidBrush(Color.Black),  xmax,0);              
            g.DrawString( (ymax).ToString(),new System.Drawing.Font("Arial", 8), new SolidBrush(Color.Black), 0, -ymax);

            
        }

        private void colorDialog1_HelpRequest(object sender, EventArgs e)
        {

        }
        private void btnColorLine_Click(object sender, EventArgs e)
        {

            btnColorLine.BackColor = colorDialog1.Color;
            if (dlg.ShowDialog() == DialogResult.OK)

                btnColorLine.BackColor = dlg.Color;
        }

        private void btnColor_Click(object sender, EventArgs e)
        {
                btnColor.BackColor = colorDialog1.Color;
          
                if (dlg.ShowDialog() == DialogResult.OK)  
                    
                    btnColor.BackColor = dlg.Color; 
           }

        private void pctDraw_Click_1(object sender, EventArgs e)
        {
            }

        private void txtLine_TextChanged(object sender, EventArgs e)
        {

        }

        private void hcrLine_Scroll(object sender, ScrollEventArgs e)
        {
            lblGage.Text = String.Concat("толщина линии:",(hcrLine.Value).ToString());
        }
        
    }     

        }
  


# -
