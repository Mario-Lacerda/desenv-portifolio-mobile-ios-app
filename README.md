# Desafio Dio - Desenvolva Seu Portfólio Mobile Criando Apps iOS



### **Projeto: Criando um Aplicativo de Conversão de Moedas para iOS**

### **Programas e Códigos:**

#### **1. Instalação do Xcode**

O Xcode é a ferramenta de desenvolvimento oficial da Apple para criar aplicativos iOS. Você pode baixá-lo gratuitamente no site da Apple.



#### **2. Criação de um novo projeto**

Para criar um novo projeto, abra o Xcode e selecione **File > New > Project**. Na janela que aparece, selecione **Single View App** e clique em **Next**.



#### **3. Definição do nome e do idioma do projeto**

Na janela que aparece, digite o nome do seu projeto e selecione o idioma que você deseja usar. Clique em **Next** para continuar.



#### **4. Seleção do tipo de dispositivo**

Na janela que aparece, selecione o tipo de dispositivo para o qual você deseja criar o aplicativo. Você pode escolher entre iPhone, iPad ou iPod touch. Clique em **Next** para continuar.



#### **5. Seleção do template**

Na janela que aparece, selecione o template **Empty App**. Clique em **Next** para continuar.



#### **6. Criação do projeto**

O Xcode criará o projeto e abrirá o arquivo de código-fonte. Você pode começar a editar o código-fonte para criar o seu aplicativo.



#### **7. Criação da interface do usuário**

Para criar a interface do usuário do aplicativo, você pode usar o **Interface Builder**. O Interface Builder é uma ferramenta gráfica que permite criar a interface do usuário do aplicativo arrastando e soltando elementos da interface.

Para abrir o Interface Builder, clique no arquivo **Main.storyboard** no navegador de projetos.



#### **8. Adição de um campo de texto**

Para adicionar um campo de texto à interface do usuário, arraste e solte um **UITextField** do **Object Library** para a tela.



#### **9. Adição de um rótulo**

Para adicionar um rótulo à interface do usuário, arraste e solte um **UILabel** do **Object Library** para a tela.



#### **10. Configuração dos elementos da interface do usuário**

Você pode configurar os elementos da interface do usuário selecionando-os no **Interface Builder** e alterando as suas propriedades no **Inspector**.



#### **11. Criação da lógica do aplicativo**

Para criar a lógica do aplicativo, você pode usar a linguagem de programação Swift. O código Swift é escrito no arquivo **ViewController.swift**.



#### **12. Importação das bibliotecas necessárias**

Para importar as bibliotecas necessárias, adicione o seguinte código ao início do arquivo **ViewController.swift**:

swift

```swift
import UIKit
```



#### **13. Definição das variáveis**

Para definir as variáveis, adicione o seguinte código ao arquivo **ViewController.swift**:

swift

```swift
var valor: Double = 0.0
var moedaOrigem: String = "USD"
var moedaDestino: String = "BRL"
```



#### **14. Criação da função para converter o valor**

Para criar a função para converter o valor, adicione o seguinte código ao arquivo **ViewController.swift**:

swift

```swift
func converterValor() {
    // Recupera o valor digitado pelo usuário
    valor = Double(valorTextField.text!)!

    // Recupera a moeda de origem
    moedaOrigem = moedaOrigemTextField.text!

    // Recupera a moeda de destino
    moedaDestino = moedaDestinoTextField.text!

    // Cria uma URL para a API de conversão de moedas
    let url = URL(string: "https://api.exchangeratesapi.io/latest?base=\(moedaOrigem)&symbols=\(moedaDestino)")!

    // Cria uma tarefa para recuperar os dados da API
    let task = URLSession.shared.dataTask(with: url) { (data, response, error) in
        if let data = data {
            // Decodifica os dados JSON
            let json = try! JSONDecoder().decode(CurrencyResponse.self, from: data)

            // Recupera a taxa de câmbio
            let taxa = json.rates[moedaDestino]!

            // Converte o valor
            let valorConvertido = valor * taxa

            // Exibe o valor convertido
            resultadoLabel.text = "\(valorConvertido) \(moedaDestino)"
        }
    }

    // Executa a tarefa
    task.resume()
}
```



#### **15. Conexão dos elementos da interface do usuário com o código**

Para conectar os elementos da interface do usuário com o código, adicione o seguinte código ao arquivo **ViewController.swift**:

swift

```swift
@IBOutlet weak var valorTextField: UITextField!
@IBOutlet weak var moedaOrigemTextField: UITextField!
@IBOutlet weak var moedaDestinoTextField: UITextField!
@IBOutlet weak var resultadoLabel: UILabel!
```



#### **16. Criação do botão para converter o valor**

Para criar o botão para converter o valor, arraste e solte um **UIButton** do **Object Library** para a tela.



#### **17. Configuração do botão**

Você pode configurar o botão selecionando-o no **Interface Builder** e alterando as suas propriedades no **Inspector**.



#### **18. Conexão do botão com o código**

Para conectar o botão com o código, adicione o seguinte código ao arquivo **ViewController.swift**:

swift

```swift
@IBAction func converterButton(_ sender: Any) {
    converterValor()
}
```



#### **19. Compilação e execução do aplicativo**

Para compilar e executar o aplicativo, selecione **Product > Build**. O Xcode compilará o aplicativo e o instalará no simulador de iOS. Você pode usar o simulador para testar o seu aplicativo.



### **Resultado:**

Ao final deste projeto, você terá criado um aplicativo iOS que converte valores entre diferentes moedas. Você pode usar este projeto como base para criar outros aplicativos iOS mais complexos.





### **Projeto: Desenvolvendo um Aplicativo de Conversão de Moedas e Câmbio para iOS**

#### **Introdução:**

Neste projeto, vamos criar um aplicativo iOS que permita aos usuários converter valores entre diferentes moedas e taxas de câmbio. O aplicativo usará a API de taxas d



### **Códigos adicionais para o aplicativo de conversão de moedas:**

#### **ViewController.swift:**

swift

```swift
import UIKit
import CurrencyAPI

class ViewController: UIViewController {

    @IBOutlet weak var valueTextField: UITextField!
    @IBOutlet weak var sourceCurrencyPickerView: UIPickerView!
    @IBOutlet weak var destinationCurrencyPickerView: UIPickerView!
    @IBOutlet weak var resultLabel: UILabel!
    @IBOutlet weak var convertButton: UIButton!

    let currencies = ["USD", "BRL", "EUR", "GBP", "JPY"]
    var sourceCurrency: String = "USD"
    var destinationCurrency: String = "BRL"

    override func viewDidLoad() {
        super.viewDidLoad()

        sourceCurrencyPickerView.dataSource = self
        sourceCurrencyPickerView.delegate = self
        destinationCurrencyPickerView.dataSource = self
        destinationCurrencyPickerView.delegate = self
    }

    @IBAction func convertButtonTapped(_ sender: Any) {
        guard let value = Double(valueTextField.text!) else {
            return
        }

        CurrencyAPI.convert(value: value, sourceCurrency: sourceCurrency, destinationCurrency: destinationCurrency) { result in
            switch result {
            case .success(let convertedValue):
                resultLabel.text = "\(convertedValue) \(destinationCurrency)"
            case .failure(let error):
                print(error)
            }
        }
    }
}

extension ViewController: UIPickerViewDataSource, UIPickerViewDelegate {

    func numberOfComponents(in pickerView: UIPickerView) -> Int {
        return 1
    }

    func pickerView(_ pickerView: UIPickerView, numberOfRowsInComponent component: Int) -> Int {
        return currencies.count
    }

    func pickerView(_ pickerView: UIPickerView, titleForRow row: Int, forComponent component: Int) -> String? {
        return currencies[row]
    }

    func pickerView(_ pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int) {
        if pickerView == sourceCurrencyPickerView {
            sourceCurrency = currencies[row]
        } else if pickerView == destinationCurrencyPickerView {
            destinationCurrency = currencies[row]
        }
    }
}
```



#### **CurrencyAPI.swift:**

swift

```swift
import Foundation

class CurrencyAPI {

    static func convert(value: Double, sourceCurrency: String, destinationCurrency: String, completion: @escaping (Result<Double, Error>) -> Void) {
        let urlString = "https://api.exchangeratesapi.io/latest?base=\(sourceCurrency)&symbols=\(destinationCurrency)"
        guard let url = URL(string: urlString) else {
            completion(.failure(NSError(domain: "CurrencyAPIErrorDomain", code: 1, userInfo: nil)))
            return
        }

        let task = URLSession.shared.dataTask(with: url) { data, response, error in
            if let error = error {
                completion(.failure(error))
                return
            }

            guard let data = data else {
                completion(.failure(NSError(domain: "CurrencyAPIErrorDomain", code: 2, userInfo: nil)))
                return
            }

            do {
                let json = try JSONDecoder().decode(CurrencyResponse.self, from: data)
                guard let rate = json.rates[destinationCurrency] else {
                    completion(.failure(NSError(domain: "CurrencyAPIErrorDomain", code: 3, userInfo: nil)))
                    return
                }
                completion(.success(value * rate))
            } catch {
                completion(.failure(error))
            }
        }

        task.resume()
    }
}

struct CurrencyResponse: Decodable {
    let rates: [String: Double]
}
```



### **Nota:**

Este código assume que você criou uma classe `CurrencyAPI` para gerenciar as chamadas de API. Você pode adaptar o código para usar sua própria implementação de API.
