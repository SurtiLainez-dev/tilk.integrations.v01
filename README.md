
# Consideraciones técnicas para conertar TILK(programa de Surtidora Lainez) con Banco Atlantida

Programa que usa Surtidora Lainez para su infraestructura de negocio completa se llama TILK. Este programa fue desarrollado por Surtidor Lainez, el cual mantiene todo su back-end ejecutandose como una API en servidores dedicados. 
Actualmente esta es nuestra primera vez que dariamos acceso a otra entidad  a nuestra API y aun estamos desarrollando nuestros metodos de seguridad para lograr que la conexion a nuestra API se la correcta y la mas segurar posibles



## API Reference

Actualmente estamos en desarrollo para aún para que un tercero tenga acceso a nuestra API, pero contamos con esta especificaciones y requerimientos necesarios(los cuales pueden ir cambiando con el desarrollo).

1. Para la conexión a la API de Tilk Surtidora Lainez les brindará un token
2. Proporcianamos rutas de nuestra API(Actualmente aun estan en desarollo para un test tienen que solicitarlo 10 días antes para programar)
#### Obtener token de nueva consulta

```http
  POST /tilk/api/integrations/v1.0/get_token
```

| Parameter | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `api_key` | `string` | **Required**. Tu API key |

return token_consulta

#### Consultar información de pago

```http
  POST /tilk/api/integrations/v1.0/get_data
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `api_key`      | `string` | **Required**. Tu API key |
| `token_consulta`      | `string` | **Required**. Token para la consulta |
| `identidad`      | `string` | **Required**. Identidad del cliente *13 caracteres |
| `tipo_pago`      | `string` | **Required**. otro_pago  or abono_cuenta |
| `num_cuenta`      | `string` | **Requierd only forma_pago !== otro_pago**. # de cuenta *num_cuenta.length >= 7 || num_cuenta.length <= 16 |

if tipo_pago === 'abono_cuenta'
return{
    saldo_actual<double>: Es el saldo total de la cuenta,
    saldo_capital<double>: Es el saldo capital de la cuenta,
    intereses_moratorios<double>:Es el total de intereses moratorios acumulados de la cuenta,
    saldo_mora<double>: Es el saldo capital que esta en mora,
    saldo_dia<double>: Es el saldo que ocupa abonar el cliente para ponerse al dia,
    proximo_ago<string>: Es el siguiente pago que le toca abonar,
    fecha_proximo_pago<date>: Es la fecha del proximo pago pendiente,
    id_cuenta<id, pendiente decidir inscriptacion a string>: Id de la cuenta
}
else if (tipo_pago === 'otro_pago')
return {
    dxcs<Array>:[{id:int, nombre:string}]
}


```http
  POST /tilk/api/integrations/v1.0/pay/register
```

| Parameter | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `api_key` | `string` | **Required**. Tu API key |
| `token_consulta`      | `string` | **Required**. Token para la consulta |
| `total`      | `double` | **Required**. Es el total que el cliente abonara |
| `email`      | `string` | **No Required**. Correo donde se le enviara el recibo de pago de parte de Surtidora Lainez |

return ok


Actualmente estas son nuestras endpoints que aún estan en desarrollo por eso aún no tenemos IP para test, pero lo mas seguro es que la initial endpoint será  https://grupolainez.com 
