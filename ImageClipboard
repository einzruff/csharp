// Drag drop an image onto the form, save filetype as jpeg, write file, resize image in memory, copy to clipboard to be pasted in other applications.
// (Example: change filetype of an webp image to jpeg, save to filesystem as jpeg, resize and copy to clipboard). 
// Uses ImageProcessor library (nuget: Install-Package ImageProcessor.Plugins.WebP -Version 1.2.0.100)

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using ImageProcessor;
using ImageProcessor.Plugins.WebP;
using ImageProcessor.Imaging.Formats;

namespace ImageClipboard
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            this.AllowDrop = true;
            pictureBox1.AllowDrop = true;
        }
        
        private void pictureBox1_DragDrop(object sender, DragEventArgs e)
        {
            string[] files = (string[])e.Data.GetData(DataFormats.FileDrop);
            foreach (string file in files) Console.WriteLine(file);

            pictureBox1.Image = (Bitmap)e.Data.GetData(DataFormats.Bitmap, true);
        }

        private void Form1_DragEnter(object sender, DragEventArgs e)
        {
            if (e.Data.GetDataPresent(DataFormats.FileDrop)) e.Effect = DragDropEffects.Copy;
        }

        public System.Drawing.Image SwapClipboardImage(System.Drawing.Image replacementImage)
        {
            System.Drawing.Image returnImage = null;
            if (Clipboard.ContainsImage())
            {
                returnImage = Clipboard.GetImage();
                Clipboard.SetImage(replacementImage);
            }
            else
            {
                Clipboard.SetImage(replacementImage);
            }
            return returnImage;
        }

        private void Form1_DragDrop(object sender, DragEventArgs e)
        {
            string[] files = (string[])e.Data.GetData(DataFormats.FileDrop);
            foreach (string file in files)
            {
                byte[] source = File.ReadAllBytes(file);
                using (MemoryStream inStream = new MemoryStream(source))
                using (MemoryStream outStream = new MemoryStream())
                using (ImageFactory imageFactory = new ImageFactory())
                {
                    // Load from various image filetypes and save as type jpeg
                    imageFactory.Load(inStream)
                                .Format(new JpegFormat())
                                .Save(outStream);

                    //save the jpeg filetype to the filesystem
                    imageFactory.Load(inStream)
                                .Format(new JpegFormat())
                                .Save(file);

                    byte[] newImage = outStream.ToArray();
                    Bitmap ff;
                    using (var ms = new MemoryStream(newImage))
                    {
                        ff = new Bitmap(ms);

                        // resize to 150, 150
                        Bitmap resized = new Bitmap(ff, new Size(150, 150));

                        Console.WriteLine(file);
                        pictureBox1.Image = new Bitmap(resized);

                        SwapClipboardImage(resized);
                    }
                }
            }
        }
    }
}
