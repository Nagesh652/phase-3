# phase-3
experiential learning
import tkinter as tk
from tkinter import messagebox
import random

class NumberGuessingGame:
    def __init__(self, root):
        self.root = root
        self.root.title("Number Guessing Game - Phase 3")
        self.root.geometry("600x500")

        self.secret_number = 0
        self.attempts = 0
        self.max_attempts = 7
        self.score = 0

        self.create_menu()

    def clear_screen(self):
        for widget in self.root.winfo_children():
            widget.destroy()

    # ---------------- MAIN MENU ----------------

    def create_menu(self):
        self.clear_screen()

        tk.Label(
            self.root,
            text="🎯 NUMBER GUESSING GAME",
            font=("Arial", 26, "bold")
        ).pack(pady=40)

        tk.Label(
            self.root,
            text="Phase 3 - GUI Based Game",
            font=("Arial", 14)
        ).pack(pady=10)

        tk.Button(
            self.root,
            text="START GAME",
            font=("Arial", 14),
            width=20,
            command=self.select_level
        ).pack(pady=15)

        tk.Button(
            self.root,
            text="GAME RULES",
            font=("Arial", 14),
            width=20,
            command=self.show_rules
        ).pack(pady=10)

        tk.Button(
            self.root,
            text="EXIT",
            font=("Arial", 14),
            width=20,
            command=self.root.destroy
        ).pack(pady=10)

    # ---------------- DIFFICULTY ----------------

    def select_level(self):
        self.clear_screen()

        tk.Label(
            self.root,
            text="SELECT DIFFICULTY",
            font=("Arial", 24, "bold")
        ).pack(pady=40)

        tk.Button(
            self.root,
            text="EASY\n1 - 50 | 10 Attempts",
            font=("Arial", 13),
            width=25,
            height=2,
            command=lambda: self.start_game(50, 10)
        ).pack(pady=10)

        tk.Button(
            self.root,
            text="MEDIUM\n1 - 100 | 7 Attempts",
            font=("Arial", 13),
            width=25,
            height=2,
            command=lambda: self.start_game(100, 7)
        ).pack(pady=10)

        tk.Button(
            self.root,
            text="HARD\n1 - 200 | 5 Attempts",
            font=("Arial", 13),
            width=25,
            height=2,
            command=lambda: self.start_game(200, 5)
        ).pack(pady=10)

        tk.Button(
            self.root,
            text="BACK",
            command=self.create_menu
        ).pack(pady=15)

    # ---------------- START GAME ----------------

    def start_game(self, maximum, attempts):

        self.secret_number = random.randint(1, maximum)
        self.maximum = maximum
        self.max_attempts = attempts
        self.attempts = 0
        self.score = 100

        self.clear_screen()

        tk.Label(
            self.root,
            text="🎮 GUESS THE NUMBER",
            font=("Arial", 25, "bold")
        ).pack(pady=25)

        tk.Label(
            self.root,
            text=f"Guess a number between 1 and {maximum}",
            font=("Arial", 15)
        ).pack(pady=10)

        self.attempt_label = tk.Label(
            self.root,
            text=f"Attempts Left: {attempts}",
            font=("Arial", 14)
        )
        self.attempt_label.pack(pady=10)

        self.guess_entry = tk.Entry(
            self.root,
            font=("Arial", 18),
            width=15,
            justify="center"
        )
        self.guess_entry.pack(pady=15)

        tk.Button(
            self.root,
            text="GUESS",
            font=("Arial", 14, "bold"),
            width=15,
            command=self.check_guess
        ).pack(pady=8)

        tk.Button(
            self.root,
            text="💡 HINT",
            font=("Arial", 12),
            width=15,
            command=self.show_hint
        ).pack(pady=8)

        self.message = tk.Label(
            self.root,
            text="Enter your guess!",
            font=("Arial", 14)
        )
        self.message.pack(pady=15)

        self.score_label = tk.Label(
            self.root,
            text="Score: 100",
            font=("Arial", 13)
        )
        self.score_label.pack()

        tk.Button(
            self.root,
            text="QUIT",
            command=self.create_menu
        ).pack(pady=20)

    # ---------------- CHECK GUESS ----------------

    def check_guess(self):

        try:
            guess = int(self.guess_entry.get())
        except ValueError:
            messagebox.showerror(
                "Error",
                "Please enter a valid number!"
            )
            return

        if guess < 1 or guess > self.maximum:
            messagebox.showwarning(
                "Warning",
                f"Enter number between 1 and {self.maximum}"
            )
            return

        self.attempts += 1
        remaining = self.max_attempts - self.attempts

        if guess == self.secret_number:

            self.score += remaining * 20

            messagebox.showinfo(
                "🎉 You Win!",
                f"Correct Number: {self.secret_number}\n"
                f"Attempts: {self.attempts}\n"
                f"Final Score: {self.score}"
            )

            self.create_menu()
            return

        if guess < self.secret_number:
            self.message.config(
                text="📈 Too Low! Try a higher number."
            )
        else:
            self.message.config(
                text="📉 Too High! Try a lower number."
            )

        self.score -= 10

        if self.score < 0:
            self.score = 0

        self.attempt_label.config(
            text=f"Attempts Left: {remaining}"
        )

        self.score_label.config(
            text=f"Score: {self.score}"
        )

        if remaining == 0:

            messagebox.showinfo(
                "Game Over",
                f"You lost!\n"
                f"Correct Number: {self.secret_number}"
            )

            self.create_menu()

    # ---------------- HINT ----------------

    def show_hint(self):

        if self.secret_number % 2 == 0:
            parity = "The number is EVEN."
        else:
            parity = "The number is ODD."

        messagebox.showinfo(
            "💡 Hint",
            parity + "\n\n"
            "Hint use reduces your score by 5."
        )

        self.score -= 5

        if self.score < 0:
            self.score = 0

        self.score_label.config(
            text=f"Score: {self.score}"
        )

    # ---------------- RULES ----------------

    def show_rules(self):

        rules = """
NUMBER GUESSING GAME RULES

1. Computer generates a random number.
2. Select Easy, Medium or Hard level.
3. Guess the number within limited attempts.
4. You get HIGH or LOW hints.
5. You can use the Hint button.
6. Wrong guesses reduce your score.
7. Using a hint also reduces your score.
8. Guess correctly to win the game.
"""

        messagebox.showinfo(
            "Game Rules",
            rules
        )


# ---------------- RUN PROGRAM ----------------

root = tk.Tk()
game = NumberGuessingGame(root)
root.mainloop()
