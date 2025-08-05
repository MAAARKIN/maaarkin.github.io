+++
date = '2025-06-04T15:00:31-03:00'
draft = false
title = 'Princípios SOLID em Go: Roteiro de Mentoria'
+++

Este material apresenta um roteiro de mentoria sobre princípios SOLID em Go, com exemplos práticos de código, é uma excelente maneira de aprofundar o conhecimento e a aplicação de boas práticas de desenvolvimento. Este roteiro será dividido em módulos, cada um cobrindo um princípio SOLID, com exemplos de código problemático e suas soluções detalhadas.

## Roteiro de Mentoria de Desenvolvimento de Código: Princípios SOLID em Go

**Público-alvo:** Desenvolvedores Go com conhecimento básico da linguagem e interesse em melhorar a qualidade do código.

---

### Módulo 0: Introdução aos Princípios SOLID

* **Objetivo:** Entender a importância dos princípios SOLID na arquitetura de software e no desenvolvimento de código sustentável.
* **Conteúdo:**
    * O que são os princípios SOLID? (SRP, OCP, LSP, ISP, DIP)
    * Por que são importantes? (Manutenibilidade, escalabilidade, testabilidade, flexibilidade, redução de acoplamento)
    * Benefícios a longo prazo: Redução de custos, maior agilidade no desenvolvimento, menor incidência de bugs.
    * Discussão: Experiências dos participantes com código "ruim" e os desafios que isso trouxe.

---

### Módulo 1: Princípio da Responsabilidade Única (SRP - Single Responsibility Principle)

* **Objetivo:** Aprender a identificar e aplicar o SRP, garantindo que cada módulo, classe ou função tenha apenas uma razão para mudar.
* **Conteúdo:**
    * Definição do SRP: "Uma classe deve ter apenas uma razão para mudar."
    * Como identificar violações do SRP: Métodos com múltiplas responsabilidades, classes "Deus".
    * Benefícios da aplicação do SRP: Maior coesão, menor acoplamento, maior testabilidade, facilidade de manutenção.
    * **Exemplo de Código (Go): Gerenciamento de Pedidos**

        * **Problema (Código `order.go` - Problema):**
            ```go
            package main

            import (
            	"fmt"
            	"strconv"
            	"strings"
            )

            type Order struct {
            	ID    string
            	Items []string
            	Total float64
            }

            func (o *Order) CalculateTotal() {
            	// Lógica de cálculo do total
            	// (Simulação)
            	o.Total = 100.00
            	fmt.Printf("Calculating total for order %s: %.2f\n", o.ID, o.Total)
            }

            func (o *Order) SaveToDatabase() {
            	// Lógica para salvar o pedido no banco de dados
            	fmt.Printf("Saving order %s to database...\n", o.ID)
            	// Ex: INSERT INTO orders (id, items, total) VALUES (...)
            }

            func (o *Order) SendConfirmationEmail() {
            	// Lógica para enviar e-mail de confirmação
            	fmt.Printf("Sending confirmation email for order %s...\n", o.ID)
            	// Ex: smtp.SendEmail(...)
            }

            func main() {
            	order := Order{
            		ID:    "123",
            		Items: []string{"Item A", "Item B"},
            	}
            	order.CalculateTotal()
            	order.SaveToDatabase()
            	order.SendConfirmationEmail()
            }
            ```
            * **Descrição do Problema:** A estrutura `Order` e seus métodos violam o SRP. A `Order` não deveria ser responsável por calcular o total, salvar no banco de dados e enviar e-mails. Ela tem múltiplas razões para mudar: se a lógica de cálculo mudar, se o mecanismo de persistência mudar, ou se o serviço de e-mail mudar. Isso a torna menos coesa e mais difícil de manter.

        * **Solução (Código `order_solution.go` - Solução):**
            ```go
            package main

            import (
            	"fmt"
            )

            // Order representa um pedido, com sua responsabilidade principal de conter os dados do pedido.
            type Order struct {
            	ID    string
            	Items []string
            	Total float64
            }

            // OrderCalculator é responsável por calcular o total de um pedido.
            type OrderCalculator struct{}

            func (oc *OrderCalculator) CalculateTotal(order *Order) {
            	// Lógica de cálculo do total
            	// (Simulação)
            	order.Total = 100.00
            	fmt.Printf("Calculating total for order %s: %.2f\n", order.ID, order.Total)
            }

            // OrderRepository é responsável por persistir pedidos.
            type OrderRepository struct{}

            func (or *OrderRepository) Save(order *Order) {
            	// Lógica para salvar o pedido no banco de dados
            	fmt.Printf("Saving order %s to database...\n", order.ID)
            	// Ex: INSERT INTO orders (id, items, total) VALUES (...)
            }

            // EmailService é responsável por enviar e-mails.
            type EmailService struct{}

            func (es *EmailService) SendConfirmationEmail(order *Order) {
            	// Lógica para enviar e-mail de confirmação
            	fmt.Printf("Sending confirmation email for order %s...\n", order.ID)
            	// Ex: smtp.SendEmail(...)
            }

            func main() {
            	order := &Order{
            		ID:    "123",
            		Items: []string{"Item A", "Item B"},
            	}

            	calculator := &OrderCalculator{}
            	calculator.CalculateTotal(order)

            	repo := &OrderRepository{}
            	repo.Save(order)

            	emailService := &EmailService{}
            	emailService.SendConfirmationEmail(order)
            }
            ```
            * **Descrição da Solução:**
                * A estrutura `Order` agora tem apenas a responsabilidade de manter os dados de um pedido.
                * `OrderCalculator` foi criada para ter a responsabilidade única de calcular o total do pedido. Se a lógica de cálculo mudar, apenas essa estrutura precisa ser modificada.
                * `OrderRepository` foi criada para ter a responsabilidade única de persistir pedidos. Se o mecanismo de persistência (ex: banco de dados, NoSQL) mudar, apenas essa estrutura é afetada.
                * `EmailService` foi criada para ter a responsabilidade única de enviar e-mails. Se o provedor de e-mail ou a lógica de envio mudar, apenas essa estrutura será modificada.
                * No `main`, as responsabilidades são delegadas a instâncias separadas, demonstrando a separação de preocupações. Isso resulta em código mais modular, testável e fácil de manter.

* **Exercícios Práticos:**
    * Identificar violações do SRP em trechos de código existentes (fornecidos pelo mentor ou trazidos pelos participantes).
    * Refatorar um trecho de código para aplicar o SRP.
* **Discussão:** Desafios na aplicação do SRP, exemplos reais de seu impacto.

---

### Módulo 2: Princípio do Aberto/Fechado (OCP - Open/Closed Principle)

* **Objetivo:** Entender como projetar software que possa ser estendido sem modificação, usando interfaces e polimorfismo.
* **Conteúdo:**
    * Definição do OCP: "Entidades de software (classes, módulos, funções, etc.) devem ser abertas para extensão, mas fechadas para modificação."
    * Como identificar violações do OCP: Condicionais extensos (if-else if, switch-case) para lidar com novos tipos, modificações constantes em código existente para adicionar novas funcionalidades.
    * Benefícios da aplicação do OCP: Redução de bugs, maior reusabilidade, facilidade de adicionar novas funcionalidades.
    * **Exemplo de Código (Go): Cálculo de Salário**

        * **Problema (Código `salary.go` - Problema):**
            ```go
            package main

            import "fmt"

            type Employee struct {
            	Name string
            	Type string // "full-time", "part-time", "contractor"
            }

            func CalculateSalary(emp Employee) float64 {
            	switch emp.Type {
            	case "full-time":
            		return 5000.00
            	case "part-time":
            		return 2500.00
            	case "contractor":
            		return 6000.00 // Exemplo de cálculo fixo
            	default:
            		return 0.00
            	}
            }

            func main() {
            	ftEmployee := Employee{Name: "Alice", Type: "full-time"}
            	ptEmployee := Employee{Name: "Bob", Type: "part-time"}
            	contractor := Employee{Name: "Charlie", Type: "contractor"}

            	fmt.Printf("Salary for %s (%s): %.2f\n", ftEmployee.Name, ftEmployee.Type, CalculateSalary(ftEmployee))
            	fmt.Printf("Salary for %s (%s): %.2f\n", ptEmployee.Name, ptEmployee.Type, CalculateSalary(ptEmployee))
            	fmt.Printf("Salary for %s (%s): %.2f\n", contractor.Name, contractor.Type, CalculateSalary(contractor))

            	// E se adicionarmos um novo tipo de funcionário, como "intern"?
            	// Teríamos que modificar a função CalculateSalary.
            }
            ```
            * **Descrição do Problema:** A função `CalculateSalary` viola o OCP. Se um novo tipo de funcionário for adicionado (ex: "intern"), a função `CalculateSalary` precisa ser modificada, adicionando mais um `case` no `switch`. Isso significa que o código está fechado para extensão (não podemos adicionar novos tipos sem modificá-lo) e aberto para modificação (precisamos modificá-lo para adicionar novos tipos).

        * **Solução (Código `salary_solution.go` - Solução):**
            ```go
            package main

            import "fmt"

            // SalaryCalculator define a interface para cálculo de salário.
            type SalaryCalculator interface {
            	CalculateSalary() float64
            }

            // FullTimeEmployee representa um funcionário em tempo integral.
            type FullTimeEmployee struct {
            	Name string
            }

            func (fte *FullTimeEmployee) CalculateSalary() float64 {
            	return 5000.00
            }

            // PartTimeEmployee representa um funcionário em tempo parcial.
            type PartTimeEmployee struct {
            	Name string
            }

            func (pte *PartTimeEmployee) CalculateSalary() float64 {
            	return 2500.00
            }

            // Contractor representa um contratado.
            type Contractor struct {
            	Name string
            }

            func (c *Contractor) CalculateSalary() float64 {
            	return 6000.00
            }

            // NewEmployeeType representa um novo tipo de funcionário adicionado posteriormente.
            type NewEmployeeType struct {
            	Name string
            }

            func (net *NewEmployeeType) CalculateSalary() float64 {
            	return 3500.00 // Salário específico para este novo tipo
            }

            func main() {
            	employees := []SalaryCalculator{
            		&FullTimeEmployee{Name: "Alice"},
            		&PartTimeEmployee{Name: "Bob"},
            		&Contractor{Name: "Charlie"},
            		&NewEmployeeType{Name: "David"}, // Adicionando um novo tipo de funcionário sem modificar o código existente
            	}

            	for _, emp := range employees {
            		// O tipo concreto do funcionário é determinado em tempo de execução
            		// através da interface SalaryCalculator.
            		fmt.Printf("Salary for %s: %.2f\n", emp.(fmt.Stringer).String(), emp.CalculateSalary())
            	}
            }

            // Helper para imprimir o nome dos funcionários (para o exemplo)
            func (fte *FullTimeEmployee) String() string {
            	return fte.Name
            }

            func (pte *PartTimeEmployee) String() string {
            	return pte.Name
            }

            func (c *Contractor) String() string {
            	return c.Name
            }

            func (net *NewEmployeeType) String() string {
            	return net.Name
            }
            ```
            * **Descrição da Solução:**
                * Foi criada uma interface `SalaryCalculator` que define o método `CalculateSalary()`.
                * Cada tipo de funcionário (`FullTimeEmployee`, `PartTimeEmployee`, `Contractor`) implementa essa interface, encapsulando a lógica de cálculo de seu próprio salário.
                * A função `main` (ou qualquer outra função que precise calcular o salário) agora opera sobre a interface `SalaryCalculator`, em vez de tipos concretos de `Employee`.
                * Quando um novo tipo de funcionário (ex: `NewEmployeeType`) precisa ser adicionado, basta criar uma nova estrutura que implemente a interface `SalaryCalculator`. Não é necessário modificar a função `main` ou qualquer outra função que utilize a interface. O código está "aberto para extensão" (podemos adicionar novos tipos de cálculo) e "fechado para modificação" (não precisamos mudar o código existente para isso).

* **Exercícios Práticos:**
    * Refatorar um sistema de relatórios para permitir novos tipos de relatórios sem modificar o código principal.
    * Discutir cenários onde o OCP pode ser aplicado para evitar `if-else if` ou `switch-case` gigantes.
* **Discussão:** Como interfaces e polimorfismo são essenciais para o OCP em Go.

---

### Módulo 3: Princípio da Substituição de Liskov (LSP - Liskov Substitution Principle)

* **Objetivo:** Garantir que subtipos possam ser substituídos por seus tipos base sem alterar a corretude do programa.
* **Conteúdo:**
    * Definição do LSP: "Se para cada objeto `o1` do tipo `S` existe um objeto `o2` do tipo `T` tal que, para todos os programas `P` definidos em termos de `T`, o comportamento de `P` é inalterado quando `o1` é substituído por `o2`, então `S` é um subtipo de `T`." (Barbara Liskov)
    * Como identificar violações do LSP: Subtipos que lançam exceções inesperadas, subtipos que retornam valores nulos onde o tipo base não retornaria, subtipos que alteram o comportamento esperado do contrato.
    * Benefícios da aplicação do LSP: Maior robustez, previsibilidade do sistema, facilidade de refatoração.
    * **Exemplo de Código (Go): Processamento de Arquivos**

        * **Problema (Código `file_processor.go` - Problema):**
            ```go
            package main

            import (
            	"fmt"
            	"io"
            	"os"
            	"strings"
            )

            // FileReader é uma interface para leitura de dados.
            type FileReader interface {
            	Read(p []byte) (n int, err error)
            	Close() error
            }

            // SimpleFileReader implementa FileReader para ler arquivos locais.
            type SimpleFileReader struct {
            	file *os.File
            }

            func NewSimpleFileReader(path string) (*SimpleFileReader, error) {
            	file, err := os.Open(path)
            	if err != nil {
            		return nil, err
            	}
            	return &SimpleFileReader{file: file}, nil
            }

            func (sfr *SimpleFileReader) Read(p []byte) (n int, err error) {
            	return sfr.file.Read(p)
            }

            func (sfr *SimpleFileReader) Close() error {
            	return sfr.file.Close()
            }

            // CompressedFileReader (subtipo) implementa FileReader para ler arquivos comprimidos.
            // Contém uma violação do LSP.
            type CompressedFileReader struct {
            	data []byte // Simulação de dados descomprimidos
            	pos  int
            }

            func NewCompressedFileReader(path string) (*CompressedFileReader, error) {
            	// Simula descompressão de um arquivo.
            	// Para este exemplo, apenas carrega um string como "descomprimido".
            	fmt.Printf("Decompressing file %s...\n", path)
            	content := "This is a decompressed content from " + path
            	return &CompressedFileReader{data: []byte(content), pos: 0}, nil
            }

            // Read de CompressedFileReader não funciona como esperado, pois o Close() não faz sentido.
            // Além disso, sua leitura é sempre do buffer em memória, não de um stream.
            func (cfr *CompressedFileReader) Read(p []byte) (n int, err error) {
            	if cfr.pos >= len(cfr.data) {
            		return 0, io.EOF
            	}
            	n = copy(p, cfr.data[cfr.pos:])
            	cfr.pos += n
            	return n, nil
            }

            func (cfr *CompressedFileReader) Close() error {
            	// Não há nada para fechar para um arquivo "descomprimido" em memória.
            	// Mas a interface exige Close(). Isso é uma violação implícita do LSP,
            	// pois um cliente de FileReader esperaria que Close() fizesse algo significativo.
            	fmt.Println("No actual close operation for CompressedFileReader.")
            	return nil
            }

            func ProcessFile(reader FileReader) {
            	buffer := make([]byte, 10)
            	for {
            		n, err := reader.Read(buffer)
            		if err == io.EOF {
            			break
            		}
            		if err != nil {
            			fmt.Printf("Error reading: %v\n", err)
            			break
            		}
            		fmt.Printf("Read: %s\n", string(buffer[:n]))
            	}
            	err := reader.Close()
            	if err != nil {
            		fmt.Printf("Error closing: %v\n", err)
            	}
            	fmt.Println("File processing finished.")
            }

            func main() {
            	// Criando um arquivo de teste
            	testFile := "test.txt"
            	err := os.WriteFile(testFile, []byte("Hello, Go Developers!"), 0644)
            	if err != nil {
            		panic(err)
            	}
            	defer os.Remove(testFile)

            	// Exemplo com SimpleFileReader (funciona bem)
            	fmt.Println("--- Processing SimpleFileReader ---")
            	simpleReader, err := NewSimpleFileReader(testFile)
            	if err != nil {
            		fmt.Println("Error:", err)
            		return
            	}
            	ProcessFile(simpleReader)

            	// Exemplo com CompressedFileReader (comportamento inesperado ou ineficaz do Close)
            	fmt.Println("\n--- Processing CompressedFileReader ---")
            	compressedReader, err := NewCompressedFileReader("archive.zip")
            	if err != nil {
            		fmt.Println("Error:", err)
            		return
            	}
            	ProcessFile(compressedReader)
            }
            ```
            * **Descrição do Problema:** A interface `FileReader` sugere que o leitor está trabalhando com um recurso que precisa ser fechado (como um arquivo). `SimpleFileReader` se comporta conforme o esperado. No entanto, `CompressedFileReader`, ao implementar a mesma interface, tem um método `Close()` que não faz nada significativo (apenas imprime uma mensagem). Isso viola o LSP porque um cliente que espera um `FileReader` e invoca `Close()` em um `CompressedFileReader` não terá o mesmo comportamento esperado de liberar recursos, ou pode ter um comportamento ineficaz, mesmo que o código não "quebre" diretamente. A expectativa do contrato da interface não é completamente honrada pelo subtipo. Outra sutileza é que `SimpleFileReader` lida com um stream de bytes de um arquivo, enquanto `CompressedFileReader` lida com um buffer em memória, o que pode levar a suposições incorretas por parte do cliente que usa a interface.

        * **Solução (Código `file_processor_solution.go` - Solução):**
            ```go
            package main

            import (
            	"bytes"
            	"fmt"
            	"io"
            	"os"
            )

            // DataReader é uma interface mais genérica para leitura de dados.
            // Ela foca apenas na capacidade de leitura.
            type DataReader interface {
            	Read(p []byte) (n int, err error)
            }

            // Closer é uma interface para recursos que precisam ser fechados.
            type Closer interface {
            	Close() error
            }

            // ReadCloser combina as interfaces DataReader e Closer, para casos onde ambos são necessários.
            type ReadCloser interface {
            	DataReader
            	Closer
            }

            // LocalFileReader implementa ReadCloser para ler arquivos locais.
            type LocalFileReader struct {
            	file *os.File
            }

            func NewLocalFileReader(path string) (*LocalFileReader, error) {
            	file, err := os.Open(path)
            	if err != nil {
            		return nil, err
            	}
            	return &LocalFileReader{file: file}, nil
            }

            func (lfr *LocalFileReader) Read(p []byte) (n int, err error) {
            	return lfr.file.Read(p)
            }

            func (lfr *LocalFileReader) Close() error {
            	return lfr.file.Close()
            }

            // InMemoryDataReader implementa DataReader para ler dados da memória.
            // Não implementa Closer, pois não há recurso para fechar.
            type InMemoryDataReader struct {
            	reader *bytes.Reader
            }

            func NewInMemoryDataReader(data []byte) *InMemoryDataReader {
            	return &InMemoryDataReader{reader: bytes.NewReader(data)}
            }

            func (imdr *InMemoryDataReader) Read(p []byte) (n int, err error) {
            	return imdr.reader.Read(p)
            }

            // ProcessDataReader processa qualquer tipo que implemente DataReader.
            func ProcessDataReader(reader DataReader) {
            	buffer := make([]byte, 10)
            	for {
            		n, err := reader.Read(buffer)
            		if err == io.EOF {
            			break
            		}
            		if err != nil {
            			fmt.Printf("Error reading: %v\n", err)
            			break
            		}
            		fmt.Printf("Read: %s\n", string(buffer[:n]))
            	}
            	fmt.Println("Data processing finished.")
            }

            // ProcessReadCloser processa qualquer tipo que implemente ReadCloser.
            func ProcessReadCloser(reader ReadCloser) {
            	ProcessDataReader(reader) // Reutiliza a lógica de leitura
            	err := reader.Close()
            	if err != nil {
            		fmt.Printf("Error closing: %v\n", err)
            	}
            	fmt.Println("Resource closed.")
            }

            func main() {
            	// Criando um arquivo de teste
            	testFile := "test.txt"
            	err := os.WriteFile(testFile, []byte("Hello, Go Developers!"), 0644)
            	if err != nil {
            		panic(err)
            	}
            	defer os.Remove(testFile)

            	// Exemplo com LocalFileReader usando ProcessReadCloser
            	fmt.Println("--- Processing LocalFileReader ---")
            	localReader, err := NewLocalFileReader(testFile)
            	if err != nil {
            		fmt.Println("Error:", err)
            		return
            	}
            	ProcessReadCloser(localReader)

            	// Exemplo com InMemoryDataReader usando ProcessDataReader
            	fmt.Println("\n--- Processing InMemoryDataReader ---")
            	inMemoryData := []byte("This is data from memory.")
            	inMemoryReader := NewInMemoryDataReader(inMemoryData)
            	ProcessDataReader(inMemoryReader)
            }
            ```
            * **Descrição da Solução:**
                * As interfaces foram divididas para refletir responsabilidades menores e mais específicas: `DataReader` (apenas leitura) e `Closer` (apenas fechamento de recursos).
                * A interface `ReadCloser` foi introduzida como uma composição de `DataReader` e `Closer`, para quando um leitor também precisa ser fechado.
                * `LocalFileReader` agora implementa `ReadCloser`, pois lida com um arquivo que precisa ser fechado.
                * `InMemoryDataReader` (substituindo `CompressedFileReader` para um exemplo mais claro de dados em memória) implementa *apenas* `DataReader`, pois não há recurso externo para fechar.
                * As funções de processamento (`ProcessDataReader` e `ProcessReadCloser`) agora dependem da interface mais específica que precisam. `ProcessDataReader` pode trabalhar com qualquer leitor que não precise ser fechado, enquanto `ProcessReadCloser` requer um leitor que também possa ser fechado.
                * Com essa abordagem, `InMemoryDataReader` pode ser substituído por `LocalFileReader` em qualquer contexto onde apenas a leitura é necessária (`ProcessDataReader`), e o sistema não espera que `Close()` faça algo significativo quando não há recurso para fechar. Isso garante que os subtipos se comportem como esperado por seus tipos base (ou, neste caso, interfaces base), honrando os contratos.

* **Exercícios Práticos:**
    * Analisar um trecho de código e identificar possíveis violações do LSP.
    * Refatorar um código para garantir que os subtipos possam ser substituídos pelos tipos base sem problemas.
* **Discussão:** Como o LSP está relacionado à herança (ou incorporação/interfaces em Go) e à garantia de contratos.

---

### Módulo 4: Princípio da Segregação de Interfaces (ISP - Interface Segregation Principle)

* **Objetivo:** Compreender a importância de interfaces pequenas e coesas, evitando que clientes dependam de métodos que não usam.
* **Conteúdo:**
    * Definição do ISP: "Clientes não devem ser forçados a depender de interfaces que não usam."
    * Como identificar violações do ISP: Interfaces "gordas" com muitos métodos, classes implementando métodos vazios ou com lógica de erro para satisfazer uma interface.
    * Benefícios da aplicação do ISP: Menor acoplamento, maior flexibilidade, interfaces mais claras e fáceis de entender.
    * **Exemplo de Código (Go): Gerenciamento de Tarefas**

        * **Problema (Código `task_manager.go` - Problema):**
            ```go
            package main

            import "fmt"

            // Worker é uma interface "gorda" que engloba muitas funcionalidades.
            type Worker interface {
            	PerformTask()
            	ReportProgress()
            	ScheduleTask()
            	AuditWorker()
            }

            // Developer implementa a interface Worker.
            type Developer struct {
            	Name string
            }

            func (d *Developer) PerformTask() {
            	fmt.Printf("%s is coding a feature.\n", d.Name)
            }

            func (d *Developer) ReportProgress() {
            	fmt.Printf("%s is reporting progress on development.\n", d.Name)
            }

            func (d *Developer) ScheduleTask() {
            	// Desenvolvedores geralmente não agendam tarefas diretamente.
            	// Este método pode ser irrelevante ou implementado com lógica vazia.
            	fmt.Printf("%s does not typically schedule tasks directly.\n", d.Name)
            }

            func (d *Developer) AuditWorker() {
            	fmt.Printf("Auditing %s's development work.\n", d.Name)
            }

            // Manager implementa a interface Worker.
            type Manager struct {
            	Name string
            }

            func (m *Manager) PerformTask() {
            	// Gerentes não realizam tarefas de desenvolvimento diretamente.
            	fmt.Printf("%s is managing the team, not coding.\n", m.Name)
            }

            func (m *Manager) ReportProgress() {
            	fmt.Printf("%s is receiving progress reports.\n", m.Name)
            }

            func (m *Manager) ScheduleTask() {
            	fmt.Printf("%s is scheduling tasks for the team.\n", m.Name)
            }

            func (m *Manager) AuditWorker() {
            	fmt.Printf("Auditing %s's management activities.\n", m.Name)
            }

            func main() {
            	dev := &Developer{Name: "Alice"}
            	mgr := &Manager{Name: "Bob"}

            	// A função ProcessWorker espera a interface Worker
            	// mas nem todos os métodos são relevantes para ambos os tipos.
            	ProcessWorker(dev)
            	fmt.Println("---")
            	ProcessWorker(mgr)
            }

            func ProcessWorker(w Worker) {
            	w.PerformTask()
            	w.ReportProgress()
            	w.ScheduleTask()
            	w.AuditWorker()
            }
            ```
            * **Descrição do Problema:** A interface `Worker` é "gorda" e viola o ISP. Ela agrupa métodos que nem todos os tipos de "trabalhadores" realmente precisam. `Developer` e `Manager` são forçados a implementar métodos como `ScheduleTask()` (para `Developer`) ou `PerformTask()` (para `Manager`) que não são relevantes para suas responsabilidades primárias, levando a implementações vazias ou sem sentido. Isso aumenta o acoplamento entre os clientes (que dependem da interface `Worker`) e as implementações, tornando o sistema menos flexível e mais difícil de manter.

        * **Solução (Código `task_manager_solution.go` - Solução):**
            ```go
            package main

            import "fmt"

            // TaskPerformer define a capacidade de realizar uma tarefa.
            type TaskPerformer interface {
            	PerformTask()
            }

            // ProgressReporter define a capacidade de reportar progresso.
            type ProgressReporter interface {
            	ReportProgress()
            }

            // TaskScheduler define a capacidade de agendar tarefas.
            type TaskScheduler interface {
            	ScheduleTask()
            }

            // Auditor define a capacidade de auditar.
            type Auditor interface {
            	AuditWorker()
            }

            // Developer agora implementa apenas as interfaces relevantes.
            type Developer struct {
            	Name string
            }

            func (d *Developer) PerformTask() {
            	fmt.Printf("%s is coding a feature.\n", d.Name)
            }

            func (d *Developer) ReportProgress() {
            	fmt.Printf("%s is reporting progress on development.\n", d.Name)
            }

            func (d *Developer) AuditWorker() {
            	fmt.Printf("Auditing %s's development work.\n", d.Name)
            }

            // Manager agora implementa apenas as interfaces relevantes.
            type Manager struct {
            	Name string
            }

            func (m *Manager) ReportProgress() {
            	fmt.Printf("%s is receiving progress reports.\n", m.Name)
            }

            func (m *Manager) ScheduleTask() {
            	fmt.Printf("%s is scheduling tasks for the team.\n", m.Name)
            }

            func (m *Manager) AuditWorker() {
            	fmt.Printf("Auditing %s's management activities.\n", m.Name)
            }

            func main() {
            	dev := &Developer{Name: "Alice"}
            	mgr := &Manager{Name: "Bob"}

            	// Funções agora dependem de interfaces menores e mais específicas.
            	DoDevelopmentWork(dev)
            	ReportDevelopmentProgress(dev)
            	AuditDevelopmentWorker(dev)

            	fmt.Println("---")

            	ReportManagementProgress(mgr)
            	ScheduleManagementTasks(mgr)
            	AuditManagementWorker(mgr)
            }

            // DoDevelopmentWork depende apenas de TaskPerformer.
            func DoDevelopmentWork(t TaskPerformer) {
            	t.PerformTask()
            }

            // ReportDevelopmentProgress depende apenas de ProgressReporter.
            func ReportDevelopmentProgress(r ProgressReporter) {
            	r.ReportProgress()
            }

            // ScheduleManagementTasks depende apenas de TaskScheduler.
            func ScheduleManagementTasks(s TaskScheduler) {
            	s.ScheduleTask()
            }

            // AuditDevelopmentWorker e AuditManagementWorker dependem de Auditor.
            func AuditDevelopmentWorker(a Auditor) {
            	a.AuditWorker()
            }

            func AuditManagementWorker(a Auditor) {
            	a.AuditWorker()
            }
            ```
            * **Descrição da Solução:**
                * A interface `Worker` foi quebrada em interfaces menores e mais coesas: `TaskPerformer`, `ProgressReporter`, `TaskScheduler` e `Auditor`. Cada uma representa uma única capacidade ou responsabilidade.
                * `Developer` agora implementa apenas `TaskPerformer`, `ProgressReporter` e `Auditor`, pois são as funcionalidades relevantes para um desenvolvedor. Ele não é mais obrigado a implementar `ScheduleTask()`.
                * `Manager` agora implementa apenas `ProgressReporter`, `TaskScheduler` e `Auditor`, pois são as funcionalidades relevantes para um gerente. Ele não é mais obrigado a implementar `PerformTask()`.
                * As funções no `main` (e quaisquer outras funções do sistema) agora dependem das interfaces *específicas* de que precisam, em vez de uma interface genérica e "gorda". Por exemplo, `DoDevelopmentWork` aceita um `TaskPerformer`, e `ScheduleManagementTasks` aceita um `TaskScheduler`.
                * Isso reduz drasticamente o acoplamento. Se uma nova funcionalidade for adicionada que se aplica a apenas um subconjunto de trabalhadores (ex: `CodeReviewer` interface), ela pode ser adicionada como uma nova interface sem afetar as interfaces existentes ou forçar implementações desnecessárias em outras estruturas.

* **Exercícios Práticos:**
    * Refatorar uma interface monolítica em interfaces menores e mais focadas.
    * Discutir como o ISP leva a um design de API mais limpo.
* **Discussão:** Diferenças entre interfaces e abstrações em Go, e como o ISP se relaciona com ambos.

---

### Módulo 5: Princípio da Inversão de Dependência (DIP - Dependency Inversion Principle)

* **Objetivo:** Aprender a inverter as dependências, fazendo com que módulos de alto nível não dependam de módulos de baixo nível, mas sim de abstrações.
* **Conteúdo:**
    * Definição do DIP:
        1.  "Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender de abstrações."
        2.  "Abstrações não devem depender de detalhes. Detalhes devem depender de abstrações."
    * Como identificar violações do DIP: Módulos de alto nível chamando diretamente implementações concretas de módulos de baixo nível, testes unitários difíceis devido a dependências concretas.
    * Benefícios da aplicação do DIP: Maior flexibilidade, testabilidade, modularidade, facilidade de trocar implementações.
    * **Exemplo de Código (Go): Notificador de Eventos**

        * **Problema (Código `notifier.go` - Problema):**
            ```go
            package main

            import "fmt"

            // EmailSender é uma implementação concreta para enviar e-mails.
            type EmailSender struct{}

            func (es *EmailSender) SendEmail(to, subject, body string) {
            	fmt.Printf("Sending email to %s - Subject: %s - Body: %s\n", to, subject, body)
            }

            // SMSSender é uma implementação concreta para enviar SMS.
            type SMSSender struct{}

            func (ss *SMSSender) SendSMS(to, message string) {
            	fmt.Printf("Sending SMS to %s - Message: %s\n", to, message)
            }

            // EventNotifier é um módulo de alto nível que depende diretamente de implementações concretas.
            type EventNotifier struct {
            	emailSender *EmailSender // Dependência concreta
            	smsSender   *SMSSender   // Dependência concreta
            }

            func NewEventNotifier() *EventNotifier {
            	return &EventNotifier{
            		emailSender: &EmailSender{},
            		smsSender:   &SMSSender{},
            	}
            }

            func (en *EventNotifier) NotifyUserByEmail(userEmail, message string) {
            	en.emailSender.SendEmail(userEmail, "Notification", message)
            }

            func (en *EventNotifier) NotifyUserBySMS(userPhone, message string) {
            	en.smsSender.SendSMS(userPhone, message)
            }

            func main() {
            	notifier := NewEventNotifier()
            	notifier.NotifyUserByEmail("user@example.com", "Your order has been shipped!")
            	notifier.NotifyUserBySMS("123-456-7890", "Your order has been shipped!")

            	// Problema: Se quisermos trocar o provedor de e-mail ou SMS,
            	// ou adicionar um novo método de notificação (ex: push notification),
            	// precisamos modificar EventNotifier.
            	// Testes unitários para EventNotifier seriam difíceis, pois ele criaria
            	// instâncias reais de EmailSender e SMSSender.
            }
            ```
            * **Descrição do Problema:** `EventNotifier` é um módulo de alto nível que depende diretamente das implementações concretas `EmailSender` e `SMSSender`. Isso viola o DIP porque:
                1.  Módulos de alto nível (`EventNotifier`) dependem de módulos de baixo nível (`EmailSender`, `SMSSender`).
                2.  Abstrações não estão sendo usadas; os detalhes (como o e-mail ou SMS são enviados) estão acoplados ao módulo de alto nível.
                Se a lógica de envio de e-mail ou SMS mudar, ou se um novo tipo de notificação (ex: push) for adicionado, `EventNotifier` precisará ser modificado. Além disso, testar `EventNotifier` isoladamente é difícil, pois ele sempre criará instâncias reais dos remetentes.

        * **Solução (Código `notifier_solution.go` - Solução):**
            ```go
            package main

            import "fmt"

            // EmailSender é uma abstração para envio de e-mails.
            type EmailSender interface {
            	SendEmail(to, subject, body string)
            }

            // SMSSender é uma abstração para envio de SMS.
            type SMSSender interface {
            	SendSMS(to, message string)
            }

            // GmailSender é uma implementação concreta de EmailSender.
            type GmailSender struct{}

            func (gs *GmailSender) SendEmail(to, subject, body string) {
            	fmt.Printf("Sending email via Gmail to %s - Subject: %s - Body: %s\n", to, subject, body)
            }

            // TwilioSMSSender é uma implementação concreta de SMSSender.
            type TwilioSMSSender struct{}

            func (tss *TwilioSMSSender) SendSMS(to, message string) {
            	fmt.Printf("Sending SMS via Twilio to %s - Message: %s\n", to, message)
            }

            // EventNotifier agora depende de abstrações (interfaces).
            type EventNotifier struct {
            	emailSender EmailSender // Dependência de abstração
            	smsSender   SMSSender   // Dependência de abstração
            }

            // NewEventNotifier recebe as dependências via injeção.
            func NewEventNotifier(emailSender EmailSender, smsSender SMSSender) *EventNotifier {
            	return &EventNotifier{
            		emailSender: emailSender,
            		smsSender:   smsSender,
            	}
            }

            func (en *EventNotifier) NotifyUserByEmail(userEmail, message string) {
            	en.emailSender.SendEmail(userEmail, "Notification", message)
            }

            func (en *EventNotifier) NotifyUserBySMS(userPhone, message string) {
            	en.smsSender.SendSMS(userPhone, message)
            }

            func main() {
            	// As implementações concretas são criadas no ponto de entrada (main)
            	// e injetadas no módulo de alto nível.
            	gmailSender := &GmailSender{}
            	twilioSMSSender := &TwilioSMSSender{}

            	notifier := NewEventNotifier(gmailSender, twilioSMSSender)
            	notifier.NotifyUserByEmail("user@example.com", "Your order has been shipped!")
            	notifier.NotifyUserBySMS("123-456-7890", "Your order has been shipped!")

            	// Exemplo de como seria fácil trocar o provedor de SMS, por exemplo,
            	// para um novo provedor sem modificar EventNotifier:
            	type AnotherSMSSender struct{}

            	func (ass *AnotherSMSSender) SendSMS(to, message string) {
            		fmt.Printf("Sending SMS via Another Provider to %s - Message: %s\n", to, message)
            	}

            	anotherSMS := &AnotherSMSSender{}
            	notifierWithAnotherSMS := NewEventNotifier(gmailSender, anotherSMS)
            	fmt.Println("\n--- Notifier with Another SMS Sender ---")
            	notifierWithAnotherSMS.NotifyUserBySMS("987-654-3210", "Your payment was confirmed!")

            	// Testes unitários seriam fáceis, usando mocks/fakes para as interfaces:
            	// type MockEmailSender struct{}
            	// func (mes *MockEmailSender) SendEmail(to, subject, body string) { /* do nothing or record calls */ }
            	// type MockSMSSender struct{}
            	// func (mss *MockSMSSender) SendSMS(to, message string) { /* do nothing or record calls */ }
            	// mockNotifier := NewEventNotifier(&MockEmailSender{}, &MockSMSSender{})
            }
            ```
            * **Descrição da Solução:**
                * Foram criadas interfaces `EmailSender` e `SMSSender`, que são as abstrações para os serviços de notificação.
                * As implementações concretas (`GmailSender`, `TwilioSMSSender`) agora implementam essas interfaces.
                * `EventNotifier` (o módulo de alto nível) agora depende das interfaces `EmailSender` e `SMSSender`, e não das implementações concretas.
                * As dependências são injetadas no construtor `NewEventNotifier`, em vez de serem criadas dentro do `EventNotifier`. Isso é conhecido como Injeção de Dependência.
                * **Benefícios:**
                    * **Desacoplamento:** `EventNotifier` não precisa saber como o e-mail ou SMS são realmente enviados. Ele confia nas abstrações.
                    * **Flexibilidade:** Se o provedor de e-mail ou SMS mudar (ex: de Gmail para SendGrid), basta criar uma nova implementação da interface e injetá-la. `EventNotifier` não precisa ser modificado.
                    * **Testabilidade:** Para testes unitários de `EventNotifier`, é possível injetar "mocks" ou "fakes" das interfaces `EmailSender` e `SMSSender`, permitindo testar `EventNotifier` isoladamente sem enviar e-mails ou SMS reais.
                * No `main`, as instâncias concretas são criadas e passadas para `NewEventNotifier`, demonstrando que a inversão de dependência significa que a responsabilidade de criar as dependências é movida para "fora" do módulo de alto nível.

* **Exercícios Práticos:**
    * Refatorar um módulo de negócios que depende de um banco de dados para depender de uma interface de repositório.
    * Discutir as diferentes formas de injeção de dependência (construtor, setter, interface) e seus prós e contras em Go.
* **Discussão:** Como o DIP é o "pai" dos outros princípios SOLID, e como ele contribui para a arquitetura limpa.

---

### Revisão e Boas Práticas

* **Objetivo:** Consolidar o conhecimento dos princípios SOLID e discutir como aplicá-los no dia a dia.
* **Conteúdo:**
    * Resumo de cada princípio SOLID.
    * Interconexão entre os princípios (SRP e OCP muitas vezes dependem de LSP; DIP é a base para todos).
    * Quando e como aplicar SOLID: Não é uma bala de prata, mas um guia. Equilíbrio entre over-engineering e código caótico.
    * Ferramentas e técnicas para aplicar SOLID em Go: Interfaces, injeção de dependência, módulos coesos.
    * Discussão de código real: Análise de um projeto existente (se possível) e identificação de oportunidades de aplicação SOLID.
    * Recursos adicionais: Livros, artigos, comunidades.
* **Sessão de Perguntas e Respostas.**

---

**Recursos Adicionais e Bibliografia Sugerida:**

* **Livros:**
    * "Clean Code" por Robert C. Martin (Uncle Bob): Embora não seja Go-específico, os conceitos são universalmente aplicáveis.
    * "Clean Architecture" por Robert C. Martin (Uncle Bob): Aprofunda a arquitetura de software baseada em SOLID.
* **Artigos e Blogs:**
    * Artigos do próprio Robert C. Martin sobre SOLID.
    * Artigos sobre design de interfaces e boas práticas em Go.
* **Comunidades:**
    * Grupos de Go no Telegram, Slack, Discord.
    * Fóruns e comunidades online de desenvolvimento de software.

Este roteiro é flexível e pode ser ajustado com base no nível dos participantes e no tempo disponível. O mais importante é a discussão, a prática e a compreensão de como esses princípios levam a um código mais limpo, manutenível e escalável.