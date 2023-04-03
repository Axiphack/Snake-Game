using System;
using System.Drawing;
using System.Windows.Forms;

public class Form1 : Form
{
    private Rectangle m_player;
    private Rectangle m_food;
    private bool m_gameOver;

    public Form1()
    {
        m_player = new Rectangle(100, 100, 50, 50);
        m_food = new Rectangle(200, 200, 50, 50);
        m_gameOver = false;

        // Set the form's size and title.
        this.Size = new Size(400, 400);
        this.Text = "Snake Game";

        // Add a label to display the score.
        Label scoreLabel = new Label();
        scoreLabel.Text = "Score: 0";
        this.Controls.Add(scoreLabel);

        // Add a button to start the game.
        Button startButton = new Button();
        startButton.Text = "Start";
        startButton.Click += (o, e) => StartGame();
        this.Controls.Add(startButton);
    }

    private void StartGame()
    {
        // Start the game loop.
        while (!m_gameOver)
        {
            // Update the player's position.
            m_player.X += 10;

            // Check if the player has collided with the food.
            if (m_player.IntersectsWith(m_food))
            {
                // Increase the score.
                Score++;

                // Generate a new food location.
                m_food.X = (int)Math.Random() * 400;
                m_food.Y = (int)Math.Random() * 400;
            }

            // Check if the player has gone off the screen.
            if (m_player.X < 0 || m_player.X > 390 || m_player.Y < 0 || m_player.Y > 390)
            {
                // Game over.
                m_gameOver = true;
            }

            // Update the screen.
            this.Invalidate();

            // Wait for a short period of time.
            Thread.Sleep(100);
        }
    }

    private int Score { get; set; }

    protected override void OnPaint(PaintEventArgs e)
    {
        // Draw the background.
        e.Graphics.FillRectangle(Brushes.Black, this.ClientRectangle);

        // Draw the player.
        e.Graphics.DrawRectangle(Pens.Red, m_player.X, m_player.Y, m_player.Width, m_player.Height);

        // Draw the food.
        e.Graphics.DrawRectangle(Pens.Green, m_food.X, m_food.Y, m_food.Width, m_food.Height);

        // Draw the score.
        e.Graphics.DrawString("Score: " + Score, Font, Brushes.White, new Point(10, 10));
    }
}
