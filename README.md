import tkinter as tk
from tkinter import messagebox


class Calculadora:
    def __init__(self, root):
        self.root = root
        self.root.title("Calculadora")
        self.root.configure(bg="#092549")
        self.root.geometry("800x600")
        self.root.resizable(False, False)

        self.entrada = tk.Entry(root, width=17, font=("Arial", 20), borderwidth=0,
                                relief="solid", bg="#092549", fg="purple", justify="right")
        self.entrada.grid(row=0, column=0, columnspan=4, ipadx=8, ipady=10, pady=10.5)

        self.crear_botones()

    def crear_botones(self):
        botones = [
            ("C", 2), ("←", 1), ("/", 1),
            ("7", 1), ("8", 1), ("9", 1), ("*", 1),
            ("4", 1), ("5", 1), ("6", 1), ("-", 1),
            ("1", 1), ("2", 1), ("3", 1), ("+", 1),
            ("0", 1), (".", 1), ("=", 2)
        ]

        colores_botones = {
            "numero": "#a2abc5",
            "operador": "#5d667d",
            "igual": "#8089a1",
            "fondo": "#05132d",
            "texto": "#8cb5ff",
            "reset": "#000098",
            "borrar": "#0b59ab"
        }

        frame_botones = tk.Frame(self.root, bg="#092549")
        frame_botones.grid(row=1, column=0, columnspan=4, pady=10)

        fila, columna = 0, 0

        for boton, span in botones:
            color_fondo = colores_botones["operador"] if boton in ["/", "*", "-", "+", "=", "←"] else colores_botones["numero"]
            if boton == "C":
                color_fondo = colores_botones["reset"]
            elif boton == "←":
                color_fondo = colores_botones["borrar"]
            elif boton == "=":
                color_fondo = colores_botones["igual"]

            btn = tk.Button(frame_botones, text=boton, width=5 * span, height=2, font=("Arial", 20),
                            bg=color_fondo, fg=colores_botones["texto"], borderwidth=0,
                            command=lambda b=boton: self.click_boton(b))
            btn.grid(row=fila, column=columna, columnspan=span, padx=1, pady=1, sticky="nsew")

            columna += span
            if columna >= 4:
                columna = 0
                fila += 1

        for i in range(4):
            frame_botones.grid_columnconfigure(i, weight=1)
        for i in range(fila + 1):
            frame_botones.grid_rowconfigure(i, weight=1)

    def click_boton(self, valor):
        if valor == "=":
            try:
                resultado = str(eval(self.entrada.get()))
                self.entrada.delete(0, tk.END)
                self.entrada.insert(tk.END, resultado)
            except Exception:
                messagebox.showerror("Error", "Entrada no válida")
                self.entrada.delete(0, tk.END)
        elif valor == "C":
            self.entrada.delete(0, tk.END)
        elif valor == "←":
            self.entrada.delete(len(self.entrada.get()) - 1, tk.END)
        else:
            self.entrada.insert(tk.END, valor)


if __name__ == "__main__":
    root = tk.Tk()
    app = Calculadora(root)
    root.mainloop()
