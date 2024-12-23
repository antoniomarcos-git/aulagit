��#   a u l a g i t 
 
 import tkinter as tk
from tkinter import ttk
from abc import ABC, abstractmethod

# Classe abstrata Animal
class Animal(ABC):
    def __init__(self, nome, idade):
        self.nome = nome
        self.idade = idade

    @abstractmethod
    def emitir_som(self):
        pass

# Classe Cachorro
class Cachorro(Animal):
    def emitir_som(self):
        return "Au Au"

# Classe Gato
class Gato(Animal):
    def emitir_som(self):
        return "Miau"

# Lista para armazenar os animais cadastrados
animais = []

# Função para cadastrar animal
def cadastrar_animal():
    nome = entry_nome.get()
    idade = entry_idade.get()
    tipo = combo_tipo.get()
    
    if not nome or not idade or not tipo:
        label_msg['text'] = "Por favor, preencha todos os campos!"
        return

    try:
        idade = int(idade)
    except ValueError:
        label_msg['text'] = "Idade deve ser um número inteiro!"
        return

    if tipo == "Cachorro":
        animal = Cachorro(nome, idade)
    elif tipo == "Gato":
        animal = Gato(nome, idade)
    else:
        label_msg['text'] = "Selecione um tipo válido!"
        return

    animais.append(animal)
    label_msg['text'] = f"{tipo} {nome} cadastrado com sucesso!"
    atualizar_lista()

# Função para atualizar a lista de animais
def atualizar_lista():
    text_lista.delete("1.0", tk.END)
    for animal in animais:
        text_lista.insert(tk.END, f"{animal.nome} ({animal.idade} anos) - Som: {animal.emitir_som()}\n")

# Configuração da interface gráfica
root = tk.Tk()
root.title("Cadastro de Animais")

# Criando o Notebook (tabs)
notebook = ttk.Notebook(root)
notebook.pack(fill='both', expand=True)

# Aba Cadastro
frame_cadastro = ttk.Frame(notebook)
notebook.add(frame_cadastro, text="Cadastro")

# Widgets da aba Cadastro
label_nome = ttk.Label(frame_cadastro, text="Nome:")
label_nome.grid(row=0, column=0, padx=5, pady=5, sticky='w')

entry_nome = ttk.Entry(frame_cadastro)
entry_nome.grid(row=0, column=1, padx=5, pady=5)

label_idade = ttk.Label(frame_cadastro, text="Idade:")
label_idade.grid(row=1, column=0, padx=5, pady=5, sticky='w')

entry_idade = ttk.Entry(frame_cadastro)
entry_idade.grid(row=1, column=1, padx=5, pady=5)

label_tipo = ttk.Label(frame_cadastro, text="Tipo:")
label_tipo.grid(row=2, column=0, padx=5, pady=5, sticky='w')

combo_tipo = ttk.Combobox(frame_cadastro, values=["Cachorro", "Gato"])
combo_tipo.grid(row=2, column=1, padx=5, pady=5)

button_cadastrar = ttk.Button(frame_cadastro, text="Cadastrar", command=cadastrar_animal)
button_cadastrar.grid(row=3, column=0, columnspan=2, pady=10)

label_msg = ttk.Label(frame_cadastro, text="")
label_msg.grid(row=4, column=0, columnspan=2)

# Aba Lista de Animais
frame_lista = ttk.Frame(notebook)
notebook.add(frame_lista, text="Lista de Animais")

# Widgets da aba Lista
text_lista = tk.Text(frame_lista, width=50, height=20)
text_lista.pack(padx=10, pady=10)

# Inicializando a interface
root.mainloop()
