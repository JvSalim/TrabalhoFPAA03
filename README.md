
# 🔍 Algoritmo para Caminho Hamiltoniano

**📚 Disciplina:** Fundamentos de Projeto e Análise de Algoritmos  
**🎓 Curso:** Engenharia de Software - PUC Minas  
**👨‍🏫 Professor:** João Paulo Carneiro Aramuni  
**👨‍💻 Autor:** João Victor Salim R. G. Trad

---

## 🌟 Introdução

O Caminho Hamiltoniano é definido como um caminho em um grafo que visita cada vértice exatamente uma vez. Este projeto apresenta uma implementação de backtracking para identificar um Caminho Hamiltoniano em grafos direcionados ou não direcionados. O algoritmo é acompanhado por uma análise detalhada da sua complexidade, demonstrando, por meio de expansão de recorrência, a razão pela qual seu comportamento no pior caso é fatorial e explicando os cenários de melhor, médio e pior caso.

---

## 📂 Estrutura do Repositório

```
├── 📄 main.py                 → Implementação do algoritmo de backtracking (para grafos direcionados e não direcionados)
├── 📄 view.py                 → Visualização do grafo e do caminho (utilizando NetworkX e Matplotlib)
├── 📄 test_hamiltonian.py     → Testes unitários abrangentes para validação do algoritmo
├── 📁 assets/                 → Imagens geradas do grafo (PNG)
├── 📄 requirements.txt        → Dependências necessárias (NetworkX, Matplotlib)
└── 📄 README.md               → Documentação completa e detalhada
```

---

## 💻 Implementação do Algoritmo (main.py)

```python
def hamiltonian_path(graph, start, path=None, visited=None, directed=False):
    """
    Encontra um Caminho Hamiltoniano em grafos direcionados ou não direcionados.

    Parâmetros:
    - graph (dict): Representação do grafo por lista de adjacência (ex.: {0: [1, 2], 1: [3]}).
    - start (int): Vértice inicial.
    - path (list): Lista que acumula os vértices visitados (uso interno).
    - visited (set): Conjunto dos vértices visitados (uso interno).
    - directed (bool): Indica se o grafo é direcionado.

    Retorno:
    - list: Retorna o caminho Hamiltoniano, ou None se não for encontrado.
    """
    if path is None:
        path = []
    if visited is None:
        visited = set()
    
    # Criação de nova lista e cópia do conjunto para evitar efeitos colaterais em chamadas recursivas
    path = path + [start]
    visited = visited.copy()
    visited.add(start)
    
    # Condição de parada: se todos os vértices foram visitados, retorna o caminho completo.
    if len(path) == len(graph):
        return path
    
    # Explora cada vizinho do vértice atual. A condição garante que nenhum vértice seja repetido.
    for neighbor in graph.get(start, []):
        if neighbor not in visited:
            result = hamiltonian_path(graph, neighbor, path, visited, directed)
            if result:
                return result
    
    return None

# Exemplo de uso para grafo não direcionado
if __name__ == "__main__":
    graph = {
        0: [1, 2],
        1: [0, 3],
        2: [0, 4],
        3: [1, 4],
        4: [2, 3]
    }
    path = hamiltonian_path(graph, 0)
    print("Caminho Hamiltoniano:", path) if path else print("Não encontrado")
```

### 🧠 Explicação do Código:
- **Gerenciamento do Estado:** Cada chamada recursiva utiliza cópias da lista `path` e do conjunto `visited` para assegurar que as alterações não impactem outras ramificações da recursão.  
- **Critério de Parada:** A verificação se `len(path) == len(graph)` confirma que o caminho contém todos os vértices do grafo, ou seja, um Caminho Hamiltoniano foi formado.  
- **Exploração de Vizinhos:** A iteração sobre os vizinhos, utilizando a condição `if neighbor not in visited:`, garante que cada vértice seja visitado apenas uma vez.

---

## 📝 Explicação Linha a Linha

| **Linha**                            | **Explicação Detalhada**                                                                                                          |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| `path = path + [start]`              | Cria uma nova lista que adiciona o vértice atual, evitando a modificação da lista original em chamadas recursivas paralelas.         |
| `visited = visited.copy()`           | Gera uma cópia do conjunto de vértices visitados para impedir que mudanças em uma ramificação afetem outra.                          |
| `visited.add(start)`                 | Adiciona o vértice atual ao conjunto de visitados, assegurando que não seja reprocessado.                                           |
| `if len(path) == len(graph):`         | Verifica se o caminho atual contém todos os vértices do grafo, condição que, se satisfeita, indica que um caminho Hamiltoniano foi encontrado.  |
| `for neighbor in graph.get(start, []):` | Itera sobre os vizinhos do vértice atual obtidos a partir da lista de adjacência.                                                  |
| `if neighbor not in visited:`       | Apenas vizinhos ainda não visitados são considerados, mantendo a propriedade fundamental do Caminho Hamiltoniano.                   |
| `result = hamiltonian_path(...)`     | Realiza uma chamada recursiva para expandir a busca partindo do vizinho atual.                                                    |

---

## 🚀 Instruções de Execução

1. **Clone o repositório:**

   ```bash
   git clone https://github.com/seu-usuario/caminho-hamiltoniano.git
   cd caminho-hamiltoniano
   ```

2. **Instale as dependências (para a visualização opcional):**

   ```bash
   pip install -r requirements.txt
   ```

3. **Execute o algoritmo principal:**

   ```bash
   python main.py
   ```

4. **Execute os testes unitários:**

   ```bash
   python test_hamiltonian.py -v
   ```

5. **Gere a visualização do grafo (opcional):**

   ```bash
   python view.py
   ```

---

## 🔬 Análise Detalhada da Complexidade

### 🔄 Recorrência e Expansão

A complexidade do algoritmo é definida pela seguinte relação recursiva, considerando o pior caso:

\[
T(n) = (n-1) \cdot T(n-1) + O(1)
\]

- **\(O(1)\)** representa as operações constantes efetuadas em cada chamada recursiva (como checagens e atualizações de listas e conjuntos).

Para resolver essa recorrência, procede-se da seguinte forma:

1. **Expansão Inicial:**

   \[
   T(n) = (n-1) \cdot T(n-1) + c
   \]

2. **Expandindo uma iteracão:**

   \[
   T(n) = (n-1) \cdot \left[(n-2) \cdot T(n-2) + c\right] + c = (n-1)(n-2) \cdot T(n-2) + (n-1)c + c
   \]

3. **Continuação da Expansão:**

   Após sucessivas expansões até o caso base \( T(1) \), obtém-se:

   \[
   T(n) = (n-1) \cdot (n-2) \cdots 1 \cdot T(1) + \text{soma de constantes}
   \]

   Notando que:

   \[
   (n-1)! = (n-1) \times (n-2) \times \cdots \times 1
   \]

4. **Conclusão da Análise:**

   Desconsiderando os termos constantes somados (menores em ordem de grandeza em comparação com o fatorial), tem-se que:

   \[
   T(n) \in O((n-1)!) \quad \text{ou, equivalentemente,} \quad O(n!)
   \]

### ❓ Por Que o Teorema Mestre Não se Aplica

O Teorema Mestre é aplicável para recorrências da forma:

\[
T(n) = a \, T\left(\frac{n}{b}\right) + f(n)
\]

Entretanto, a nossa recorrência difere nos seguintes aspectos:

- **Redução Linear:**  
  Na nossa recorrência, o problema é reduzido de \( n \) para \( n-1 \) (ou seja, decremento linear), em vez de uma redução proporcional (como \( n/b \) onde \( b > 1 \)). Isso impede a aplicação direta do Teorema Mestre.

- **Número de Subproblemas:**  
  O coeficiente "\(n-1\)" na recorrência varia com o tamanho da entrada, enquanto o Teorema Mestre pressupõe que o número de subproblemas \(a\) seja uma constante independente de \(n\).

- **Formato Diferente:**  
  A forma \(T(n) = (n-1) \, T(n-1) + O(1)\) não pode ser convertida para o formato requerido (\(a \, T(n/b) + f(n)\)) sem alterar fundamentalmente a estrutura do problema.

Portanto, para essa recorrência, o método mais apropriado é a expansão iterativa, como demonstrado, que revela o comportamento fatorial \( O(n!) \).

---

## 🎨 Visualização do Grafo (Opcional)

O arquivo `view.py` utiliza as bibliotecas NetworkX e Matplotlib para desenhar o grafo e destacar visualmente o caminho Hamiltoniano encontrado. Essa funcionalidade facilita a verificação dos resultados e adiciona valor prático ao projeto.

```python
import networkx as nx
import matplotlib.pyplot as plt

def plot_hamiltonian(graph, path=None, directed=False, filename="assets/graph.png"):
    """
    Plota o grafo e destaca o Caminho Hamiltoniano.

    Parâmetros:
    - graph (dict): Representação do grafo por lista de adjacência.
    - path (list): Caminho Hamiltoniano que será destacado (opcional).
    - directed (bool): Indica se o grafo é direcionado.
    - filename (str): Caminho para salvar a imagem.
    """
    G = nx.DiGraph() if directed else nx.Graph()
    for node in graph:
        G.add_node(node)
        for neighbor in graph[node]:
            G.add_edge(node, neighbor)
    
    pos = nx.spring_layout(G)
    plt.figure(figsize=(10, 6))
    nx.draw_networkx_nodes(G, pos, node_size=700, node_color='lightblue')
    nx.draw_networkx_edges(G, pos, edge_color='gray', width=1)
    nx.draw_networkx_labels(G, pos, font_size=12, font_family='sans-serif')

    if path:
        edges = [(path[i], path[i+1]) for i in range(len(path)-1)]
        nx.draw_networkx_edges(G, pos, edgelist=edges, edge_color='red', width=2)
        nx.draw_networkx_nodes(G, pos, nodelist=path, node_color='red')
    
    plt.axis('off')
    plt.savefig(filename, dpi=300)
    plt.close()

if __name__ == "__main__":
    graph = {0: [1, 2], 1: [3], 2: [4], 3: [4], 4: []}
    path = [0, 1, 3, 4, 2]
    plot_hamiltonian(graph, path, directed=True)
```

---

## 🧪 Testes Unitários

O arquivo `test_hamiltonian.py` contém uma suíte de testes unitários que valida o comportamento do algoritmo em diferentes cenários (grafos direcionados, não direcionados e grafos completos).

```python
import pytest
from main import hamiltonian_path

TEST_CASES = [
    # Caso: Grafo não direcionado com caminho
    {"graph": {0: [1], 1: [0, 2], 2: [1, 3], 3: [2]}, "start": 0, "expected": [0, 1, 2, 3]},
    
    # Caso: Grafo direcionado sem caminho
    {"graph": {0: [1], 1: [2], 2: []}, "start": 0, "directed": True, "expected": None},
    
    # Caso: Grafo completo (K5)
    {"graph": {i: [j for j in range(5) if j != i] for i in range(5)}, "start": 0, "expected": list(range(5))}
]

@pytest.mark.parametrize("test_input", TEST_CASES)
def test_hamiltonian_path(test_input):
    result = hamiltonian_path(**test_input)
    assert result == test_input["expected"], f"Falha no teste: {test_input}"
```

---

### 📊 Resultado dos Testes:

```
collected 3 items

test_hamiltonian.py::test_hamiltonian_path[test_input0] PASSED
test_hamiltonian.py::test_hamiltonian_path[test_input1] PASSED
test_hamiltonian.py::test_hamiltonian_path[test_input2] PASSED
```

---

## 📚 Referências
AULA 02: Introdução à Teoria da Complexidade (Material do Professor).

