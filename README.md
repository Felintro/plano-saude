# plano-saude-database

Uma agência de planos de saúde deseja automatizar seu sistema de cadastro de segurados. Cada cliente deve informar os dados dados pessoais (nome, data de nascimento e email) e pode possuir diversos dependentes. Cada um dos segurados deve estar associado a um contrato (que possui os segurados, data de início da vigência e os produtos associados). Cada produto (plano de saúde) possui um código de registro na ANS, uma descrição e o valor.

```sql
CREATE TABLE cliente (
    id_cliente int not null primary key generated always as identity,
    ds_nome varchar(100), 
    dt_nascimento date,
    ds_email varchar(100)
)

CREATE TABLE produto (
    id_produto int not null primary key identity generated always as identity,
    cd_produto_ans int not null unique,
    ds_produto varchar(100),
    vl_produto money
)

CREATE TABLE contrato (
    id_contrato int not null primary key identity generated always as identity,
    dt_inicio date not null,
	id_produto int not null,
	id_cliente int not null
	constraint cliente_fk foreign key (id_cliente) references cliente(id_cliente),
	constraint produto_fk foreign key (id_produto) references produto(id_produto)
)

CREATE TABLE dependente (
    id_dependente int not null primary key identity generated always as identity,
    id_titular int not null,
    id_dependente int not null,
    id_contrato int not null,
    constraint titular_fk foreign key (id_titular) references cliente(id),
    constraint dependente_fk foreign key (id_dependente) references cliente(id),
    constraint contrato_fk foreign key (id_contrato) references contrato(id),
    constraint dependente_unique unique(id_titular, id_dependente)
)
```
