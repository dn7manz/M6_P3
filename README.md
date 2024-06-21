# Práctica 3: DID Method

Esta práctica consiste en la generación de los modelos de datos asociados a un flujo completo de emisión y verificación de credenciales. El objetivo es tener una visión general del manejo de las estructuras de datos involucradas.

## Descripción del Caso de Uso

En esta práctica, hemos definido un caso de uso donde una universidad emite una credencial verificable (VC) de título universitario a un estudiante. Luego, el estudiante utiliza esta credencial para crear una presentación verificable (VP).

### Estructuras de Datos

A continuación se presentan las estructuras de datos utilizadas en este caso de uso.

### DID Document del Emisor (Universidad)

```json
{
  "id": "did:example:university123",  // Identificador descentralizado (DID) único de la universidad.
  "publicKey": [{  // Arreglo de claves públicas asociadas con el DID.
    "id": "did:example:university123#keys-1",  // Identificador único para una clave pública específica dentro del contexto del DID.
    "type": "RsaVerificationKey2018",  // Tipo de clave pública y el algoritmo utilizado (RSA en este caso).
    "controller": "did:example:university123",  // El controlador de la clave pública (la universidad en este caso).
    "publicKeyPem": "-----BEGIN PUBLIC KEY...END PUBLIC KEY-----"  // La clave pública en formato PEM.
  }],
  "authentication": [{  // Arreglo de métodos de autenticación que pueden usarse con este DID.
    "type": "RsaSignatureAuthentication2018",  // Tipo de autenticación y el algoritmo utilizado (RSA en este caso).
    "publicKey": "did:example:university123#keys-1"  // Referencia a la clave pública específica que se usa para la autenticación.
  }]
}
```

### DID Document del titular (Estudiamte)

```json
{
  "id": "did:example:student456",  // Identificador descentralizado (DID) único del estudiante.
  "publicKey": [{  // Arreglo de claves públicas asociadas con el DID.
    "id": "did:example:student456#keys-1",  // Identificador único para una clave pública específica dentro del contexto del DID.
    "type": "Ed25519VerificationKey2018",  // Tipo de clave pública y el algoritmo utilizado (Ed25519 en este caso).
    "controller": "did:example:student456",  // El controlador de la clave pública (el estudiante en este caso).
    "publicKeyBase58": "2B5s9kF...XYZ"  // La clave pública en formato Base58.
  }],
  "authentication": [{  // Arreglo de métodos de autenticación que pueden usarse con este DID.
    "type": "Ed25519SignatureAuthentication2018",  // Tipo de autenticación y el algoritmo utilizado (Ed25519 en este caso).
    "publicKey": "did:example:student456#keys-1"  // Referencia a la clave pública específica que se usa para la autenticación.
  }]
}
```

### Credencial Verificable (VC)


```json
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",  // Contexto que define el esquema de datos para la credencial verificable.
    "https://www.w3.org/2018/credentials/examples/v1"  // Contexto adicional específico para los ejemplos de credenciales.
  ],
  "id": "http://example.edu/credentials/1872",  // Identificador único de la credencial verificable.
  "type": ["VerifiableCredential", "UniversityDegreeCredential"],  // Tipos de la credencial; es una credencial verificable y una credencial de título universitario.
  "issuer": "did:example:university123",  // DID del emisor de la credencial, en este caso, la universidad.
  "issuanceDate": "2024-06-21T19:23:24Z",  // Fecha y hora en que se emitió la credencial.
  "credentialSubject": {  // Información sobre el sujeto de la credencial, en este caso, el estudiante.
    "id": "did:example:student456",  // DID del sujeto de la credencial, el estudiante.
    "degree": {  // Detalles del grado que se otorga al estudiante.
      "type": "BachelorDegree",  // Tipo de grado (Licenciatura).
      "name": "Bachelor of Science in Computer Science"  // Nombre específico del grado.
    }
  },
  "proof": {  // Información sobre la prueba criptográfica que asegura la autenticidad de la credencial.
    "type": "RsaSignature2018",  // Tipo de firma utilizada para la prueba, en este caso RSA.
    "created": "2024-06-21T19:23:24Z",  // Fecha y hora en que se creó la prueba.
    "proofPurpose": "assertionMethod",  // Propósito de la prueba, indicando que es para la afirmación de la credencial.
    "verificationMethod": "did:example:university123#keys-1",  // Método de verificación utilizado, refiriéndose a la clave pública del emisor.
    "jws": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjI2NjU1In0.eyJzdWIiOiJkaWQ6ZXhhbXBsZTpzdHVkZW50NDU2IiwiaXNzIjoiZGlkOmV4YW1wbGU6dW5pdmVyc2l0eTEyMyIsImlhdCI6MTYxNTYzOTAyNH0.X5kD..."  // JSON Web Signature (JWS) que contiene la firma criptográfica de la prueba.
  }
}
```

### Presentación Verificable (VP)


```json
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",  // Contexto que define el esquema de datos para la credencial verificable.
    "https://www.w3.org/2018/credentials/examples/v1"  // Contexto adicional específico para los ejemplos de credenciales.
  ],
  "type": ["VerifiablePresentation"],  // Tipo de presentación verificable.
  "verifiableCredential": [  // Arreglo de credenciales verificables incluidas en la presentación.
    {
      "@context": [
        "https://www.w3.org/2018/credentials/v1",  // Contexto que define el esquema de datos para la credencial verificable.
        "https://www.w3.org/2018/credentials/examples/v1"  // Contexto adicional específico para los ejemplos de credenciales.
      ],
      "id": "http://example.edu/credentials/1872",  // Identificador único de la credencial verificable.
      "type": ["VerifiableCredential", "UniversityDegreeCredential"],  // Tipos de la credencial; es una credencial verificable y una credencial de título universitario.
      "issuer": "did:example:university123",  // DID del emisor de la credencial, en este caso, la universidad.
      "issuanceDate": "2024-06-21T19:23:24Z",  // Fecha y hora en que se emitió la credencial.
      "credentialSubject": {  // Información sobre el sujeto de la credencial, en este caso, el estudiante.
        "id": "did:example:student456",  // DID del sujeto de la credencial, el estudiante.
        "degree": {  // Detalles del grado que se otorga al estudiante.
          "type": "BachelorDegree",  // Tipo de grado (Licenciatura).
          "name": "Bachelor of Science in Computer Science"  // Nombre específico del grado.
        }
      },
      "proof": {  // Información sobre la prueba criptográfica que asegura la autenticidad de la credencial.
        "type": "RsaSignature2018",  // Tipo de firma utilizada para la prueba, en este caso RSA.
        "created": "2024-06-21T19:23:24Z",  // Fecha y hora en que se creó la prueba.
        "proofPurpose": "assertionMethod",  // Propósito de la prueba, indicando que es para la afirmación de la credencial.
        "verificationMethod": "did:example:university123#keys-1",  // Método de verificación utilizado, refiriéndose a la clave pública del emisor.
        "jws": "eyJhbGciOiJSUzI1NiIsImtpZCI6IjI2NjU1In0.eyJzdWIiOiJkaWQ6ZXhhbXBsZTpzdHVkZW50NDU2IiwiaXNzIjoiZGlkOmV4YW1wbGU6dW5pdmVyc2l0eTEyMyIsImlhdCI6MTYxNTYzOTAyNH0.X5kD..."  // JSON Web Signature (JWS) que contiene la firma criptográfica de la prueba.
      }
    }
  ],
  "proof": {  // Información sobre la prueba criptográfica que asegura la autenticidad de la presentación.
    "type": "Ed25519Signature2018",  // Tipo de firma utilizada para la prueba, en este caso Ed25519.
    "created": "2024-06-21T19:23:24Z",  // Fecha y hora en que se creó la prueba.
    "proofPurpose": "authentication",  // Propósito de la prueba, indicando que es para la autenticación de la presentación.
    "verificationMethod": "did:example:student456#keys-1",  // Método de verificación utilizado, refiriéndose a la clave pública del estudiante.
    "challenge": "123456789",  // Desafío utilizado en el proceso de autenticación para garantizar que la presentación es reciente y prevenir ataques de repetición.
    "jws": "eyJhbGciOiJFZERTQSIsImtpZCI6IjI2NjU1In0.eyJzdWIiOiJkaWQ6ZXhhbXBsZTpzdHVkZW50NDU2IiwibmJmIjoxNjE1NjM5MDI0LCJleHAiOjE2MTU2NDI2MjR9.uRkD..."  // JSON Web Signature (JWS) que contiene la firma criptográfica de la prueba.
  }
}
```

