from flask import Flask, request, jsonify

app = Flask(__name__)

def validar_cpf(cpf: str) -> bool:
    cpf = ''.join(filter(str.isdigit, cpf))
    if len(cpf) != 11 or cpf == cpf[0] * len(cpf):
        return False
    soma = sum(int(cpf[i]) * (10 - i) for i in range(9))
    digito1 = (soma * 10 % 11) % 10
    soma = sum(int(cpf[i]) * (11 - i) for i in range(10))
    digito2 = (soma * 10 % 11) % 10
    return cpf[9] == str(digito1) and cpf[10] == str(digito2)

@app.route('/validar_cpf', methods=['POST'])
def validar():
    data = request.json
    cpf = data.get('cpf', '')
    valido = validar_cpf(cpf)
    return jsonify({'valido': valido})

if __name__ == '__main__':
    app.run(debug=True)
