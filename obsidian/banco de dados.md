
### plano conceitual
![[bd_inicial.png]]

### plano lógico

medico(crm, nome, horario_entrada, horario_saida)
paciente(cpf, nome, idade, sexo)

consulta(crm, cpf, horario)

prontuario(id, detalhes, resultados, medicamentos, crm, cpf, horario_consulta)


### plano físico

```
-- Tabela Médico
CREATE TABLE medico (
    crm VARCHAR(20) PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    especialidade VARCHAR (30) NOT NULL,
    horario_entrada TIME NOT NULL,
    horario_saida TIME NOT NULL
);

-- Tabela Paciente
CREATE TABLE paciente (
    cpf VARCHAR(14) PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    idade INTEGER,
    sexo CHAR(1)
);

-- Tabela Consulta
CREATE TABLE consulta (
    crm VARCHAR(20) NOT NULL,
    cpf VARCHAR(14) NOT NULL,
    data_horario TIMESTAMP NOT NULL,
    PRIMARY KEY (crm, cpf, data_horario),
    FOREIGN KEY (crm) REFERENCES medico(crm) ON DELETE CASCADE,
    FOREIGN KEY (cpf) REFERENCES paciente(cpf) ON DELETE CASCADE
);

-- Tabela Prontuário
CREATE TABLE prontuario (
    id SERIAL PRIMARY KEY,
    detalhes TEXT,
    resultados TEXT,
    medicamentos TEXT,
    crm VARCHAR(20) NOT NULL,
    cpf VARCHAR(14) NOT NULL,
    data_horario_consulta TIMESTAMP NOT NULL,
    FOREIGN KEY (crm, cpf, data_horario_consulta) 
        REFERENCES consulta(crm, cpf, data_horario)
        ON DELETE CASCADE
);

-- Tabela Receituário
CREATE TABLE receituario (
	id SERIAL PRIMARY KEY,
	medicamentos TEXT,
	crm VARCHAR(20) NOT NULL,
    cpf VARCHAR(14) NOT NULL,
    data_horario_consulta TIMESTAMP NOT NULL,
    FOREIGN KEY (crm, cpf, data_horario_consulta) 
        REFERENCES consulta(crm, cpf, data_horario)
        ON DELETE CASCADE
);

-- Índices para melhorar performance nas consultas frequentes
CREATE INDEX idx_prontuario_paciente ON prontuario(cpf);
CREATE INDEX idx_prontuario_medico ON prontuario(crm);
```

## comandos úteis

| Comando            | Descrição                        |
| ------------------ | -------------------------------- |
| `\l`               | Lista todos os bancos de dados   |
| `\c nome_do_banco` | Conecta a um banco específico    |
| `\dt`              | Lista tabelas do banco atual     |
| `\du`              | Lista usuários e suas permissões |
| `\q`               | Sai do terminal `psql`           |
|                    |                                  |
```

psql -h localhost -U medrecord -d medrecord_db -W
```

