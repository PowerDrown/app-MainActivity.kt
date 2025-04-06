# app-MainActivity.kt
package com.example.caixaeletronico

import android.os.Bundle
import android.widget.Button
import android.widget.Toast
import androidx.appcompat.app.AlertDialog
import androidx.appcompat.app.AppCompatActivity
import com.example.caixaeletronico.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    private var saldo: Double = 0.0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.btnDepositar.setOnClickListener { depositar() }
        binding.btnSacar.setOnClickListener { sacar() }
        binding.btnConsultar.setOnClickListener { consultar() }
        binding.btnSair.setOnClickListener { finish() }
    }

    private fun depositar() {
        val builder = AlertDialog.Builder(this)
        builder.setTitle("Depósito")
        builder.setMessage("Digite o valor para depositar:")

        val input = android.widget.EditText(this)
        input.inputType = android.text.InputType.TYPE_CLASS_NUMBER or android.text.InputType.TYPE_NUMBER_FLAG_DECIMAL
        builder.setView(input)

        builder.setPositiveButton("Confirmar") { _, _ ->
            val valor = input.text.toString().toDoubleOrNull()
            if (valor != null && valor > 0) {
                saldo += valor
                Toast.makeText(this, "Depósito de R$%.2f realizado com sucesso!".format(valor), Toast.LENGTH_SHORT).show()
            } else {
                Toast.makeText(this, "Valor inválido!", Toast.LENGTH_SHORT).show()
            }
        }

        builder.setNegativeButton("Cancelar", null)
        builder.show()
    }

    private fun sacar() {
        val builder = AlertDialog.Builder(this)
        builder.setTitle("Saque")
        builder.setMessage("Digite o valor para sacar:")

        val input = android.widget.EditText(this)
        input.inputType = android.text.InputType.TYPE_CLASS_NUMBER or android.text.InputType.TYPE_NUMBER_FLAG_DECIMAL
        builder.setView(input)

        builder.setPositiveButton("Confirmar") { _, _ ->
            val valor = input.text.toString().toDoubleOrNull()
            if (valor != null && valor > 0) {
                if (valor <= saldo) {
                    saldo -= valor
                    Toast.makeText(this, "Saque de R$%.2f realizado com sucesso!".format(valor), Toast.LENGTH_SHORT).show()
                } else {
                    Toast.makeText(this, "Saldo insuficiente!", Toast.LENGTH_SHORT).show()
                }
            } else {
                Toast.makeText(this, "Valor inválido!", Toast.LENGTH_SHORT).show()
            }
        }

        builder.setNegativeButton("Cancelar", null)
        builder.show()
    }

    private fun consultar() {
        val builder = AlertDialog.Builder(this)
        builder.setTitle("Saldo")
        builder.setMessage("Seu saldo atual é: R$%.2f".format(saldo))
        builder.setPositiveButton("OK", null)
        builder.show()
    }
}
