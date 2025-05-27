dart 

import 'package:flutter/material.dart';
import 'package:simulador_financiamento/ui/financiamento_screen.dart';
&nbsp;
&nbsp;

void main() {
  runApp(const MaterialApp(
    title: 'Simulador de Financiamento',
    home: FinanciamentoScreen(),
  ));
}

financiamento_screen.dart

import 'package:flutter/material.dart';
import 'resultado_screen.dart';
&nbsp;
&nbsp;

class FinanciamentoScreen extends StatefulWidget {
  const FinanciamentoScreen({super.key});
&nbsp;
&nbsp;

  @override
  State<FinanciamentoScreen> createState() => _FinanciamentoScreenState();
}
&nbsp;
&nbsp;

class _FinanciamentoScreenState extends State<FinanciamentoScreen> {
  final TextEditingController valorController = TextEditingController();
  final TextEditingController parcelasController = TextEditingController();
  final TextEditingController taxaJurosController = TextEditingController();
  final TextEditingController taxasAdicionaisController = TextEditingController();
&nbsp;
&nbsp;

  void calcularFinanciamento() {
    double valorBem = double.tryParse(valorController.text) ?? 0.0;
    int numeroParcelas = int.tryParse(parcelasController.text) ?? 0;
    double taxaJuros = double.tryParse(taxaJurosController.text) ?? 0.0 / 100;
    double taxasAdicionais = double.tryParse(taxasAdicionaisController.text) ?? 0.0;
&nbsp;
&nbsp;

    double montante = valorBem + taxasAdicionais;
    double taxaMensal = taxaJuros / 12;
    double valorParcela = (montante * taxaMensal) / (1 - (1 + taxaMensal).pow(-numeroParcelas));
    double montanteTotal = valorParcela * numeroParcelas;
&nbsp;
&nbsp;

    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => ResultadoScreen(
          valorParcela: valorParcela,
          montanteTotal: montanteTotal,
        ),
      ),
    );
  }
&nbsp;
&nbsp;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Simulador de Financiamento'),
        backgroundColor: Colors.blueGrey,
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: [
            _buildTextField('Valor do Bem', valorController),
            _buildTextField('Número de Parcelas', parcelasController),
            _buildTextField('Taxa de Juros Mensal (%)', taxaJurosController),
            _buildTextField('Taxas Adicionais', taxasAdicionaisController),
            ElevatedButton(
              onPressed: calcularFinanciamento,
              child: Text('Calcular'),
            ),
          ],
        ),
      ),
    );
  }
&nbsp;
&nbsp;

  Widget _buildTextField(String label, TextEditingController controller) {
    return Column(
      children: [
        Text(label, style: TextStyle(fontSize: 18, color: Colors.blueGrey)),
        TextField(
          controller: controller,
          decoration: InputDecoration(
            border: OutlineInputBorder(),
            labelText: 'Digite $label',
          ),
          keyboardType: TextInputType.number,
        ),
      ],
    );
  }
}


resultado_screen.dart



import 'package:flutter/material.dart';
&nbsp;
&nbsp;

class ResultadoScreen extends StatelessWidget {
  final double valorParcela;
  final double montanteTotal;
&nbsp;
&nbsp;

  const ResultadoScreen({super.key, required this.valorParcela, required this.montanteTotal});
&nbsp;
&nbsp;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Resultado do Financiamento'),
        backgroundColor: Colors.blueGrey,
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: [
            Text('Valor da Parcela: R\$ ${valorParcela.toStringAsFixed(2)}', style: TextStyle(fontSize: 22, color: Colors.blueGrey)),
            Text('Montante Total: R\$ ${montanteTotal.toStringAsFixed(2)}', style: TextStyle(fontSize: 22, color: Colors.blueGrey)),
            ElevatedButton(
              onPressed: () {
                Navigator.pop(context);
              },
              child: Text('Voltar'),
            ),
          ],
        ),
      ),
    );
  }
}
