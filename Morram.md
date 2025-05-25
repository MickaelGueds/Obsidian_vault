# 📘 Guia de Recorrências – Expansão, Fórmula Fechada e Complexidade

Este guia apresenta os principais padrões de recorrência usados em Análise de Algoritmos. Para cada padrão, temos:

- A **forma da recorrência**
    
- A **expansão passo a passo**
    
- A **fórmula fechada**
    
- A **complexidade final Θ**
    

---

## 🧠 Tabela Resumida

1. **T(n) = T(n−1) + 1**
    
    - Expansão: T(1) + (n−1)
        
    - Fórmula fechada: T(n) = n
        
    - Complexidade: Θ(n)
        
2. **T(n) = T(n−1) + n**
    
    - Expansão: Soma de 1 até n
        
    - Fórmula fechada: T(n) = n(n+1)/2
        
    - Complexidade: Θ(n²)
        
3. **T(n) = T(n/2) + 1**
    
    - Expansão: log₂(n) somas de 1
        
    - Fórmula fechada: T(n) = 1 + log₂(n)
        
    - Complexidade: Θ(log n)
        
4. **T(n) = T(n/2) + n**
    
    - Expansão: n + n/2 + n/4 + ...
        
    - Fórmula fechada: T(n) ≈ 2n
        
    - Complexidade: Θ(n)
        
5. **T(n) = T(n/3) + n**
    
    - Expansão: n + n/3 + n/9 + ...
        
    - Fórmula fechada: T(n) ≈ 1.5n
        
    - Complexidade: Θ(n)
        
6. **T(n) = 2T(n/2) + n**
    
    - Expansão: n + n + n + ... (log₂(n) vezes)
        
    - Fórmula fechada: T(n) = n log₂(n)
        
    - Complexidade: Θ(n log n)
        
7. **T(n) = 2T(n−1)**
    
    - Expansão: 2 × 2 × 2 × ... = 2^(n−1) × T(1)
        
    - Fórmula fechada: T(n) = 2^(n−1)
        
    - Complexidade: Θ(2ⁿ)
        

---

# 📘 Guia Completo de Recorrências – Expansão, Dedução e Complexidade

Este guia apresenta os principais padrões de recorrência usados em Análise de Algoritmos. Para cada padrão, temos:

- A forma da recorrência
- A expansão detalhada
- A generalização com k
- A condição de parada
- A dedução da fórmula fechada
- A complexidade final Θ

---

## 🔢 1. T(n) = T(n − 1) + 1

- **Expansão:**
  - T(n) = T(n−1) + 1  
  - T(n−1) = T(n−2) + 1  
  - T(n−2) = T(n−3) + 1  
  - ...  
  - T(n) = T(n−k) + k

- **Parada:**
  - Quando n−k = 1 → k = n−1

- **Fórmula fechada:**
  - T(n) = T(1) + (n−1)  
  - Se T(1) = 1 → T(n) = n

- **Complexidade:**  
  - Θ(n)

---

## 🔢 2. T(n) = T(n − 1) + n

- **Expansão:**
  - T(n) = T(n−1) + n  
  - T(n−1) = T(n−2) + (n−1)  
  - T(n−2) = T(n−3) + (n−2)  
  - ...  
  - T(n) = T(n−k) + (n−k+1) + ... + n

- **Parada:**
  - Quando n−k = 0 → k = n

- **Fórmula fechada:**
  - T(n) = T(0) + 1 + 2 + ... + n  
  - Soma da PA = n(n+1)/2

- **Complexidade:**  
  - Θ(n²)

---

## 🔢 3. T(n) = T(n / 2) + 1

- **Expansão:**
  - T(n) = T(n/2) + 1  
  - T(n/2) = T(n/4) + 1  
  - T(n/4) = T(n/8) + 1  
  - ...  
  - T(n) = T(n/2^k) + k

- **Parada:**
  - Quando n / 2^k = 1 → 2^k = n → k = log₂(n)

- **Fórmula fechada:**
  - T(n) = T(1) + log₂(n)  
  - Se T(1) = 1 → T(n) = 1 + log₂(n)

- **Complexidade:**  
  - Θ(log n)

---

## 🔢 4. T(n) = T(n / 2) + n

- **Expansão:**
  - T(n) = T(n/2) + n  
  - T(n/2) = T(n/4) + n/2  
  - T(n/4) = T(n/8) + n/4  
  - ...  
  - T(n) = T(n/2^k) + n + n/2 + n/4 + ... + n/2^k

- **Parada:**
  - Quando n / 2^k = 1 → 2^k = n → k = log₂(n)

- **Fórmula fechada:**
  - Soma geométrica: n(1 + 1/2 + 1/4 + ... + 1/n)  
  - Limite da soma: < 2  
  - T(n) ≈ n × 2 = 2n

- **Complexidade:**  
  - Θ(n)

---

## 🔢 5. T(n) = T(n / 3) + n

- **Expansão:**
  - T(n) = T(n/3) + n  
  - T(n/3) = T(n/9) + n/3  
  - T(n/9) = T(n/27) + n/9  
  - ...  
  - T(n) = T(n/3^k) + n + n/3 + n/9 + ... + n/3^k

- **Parada:**
  - Quando n / 3^k = 1 → 3^k = n → k = log₃(n)

- **Fórmula fechada:**
  - Soma geométrica: n(1 + 1/3 + 1/9 + ...)  
  - Soma da PG = 1 / (1 - 1/3) = 3/2  
  - T(n) ≈ n × 3/2 = 1.5n

- **Complexidade:**  
  - Θ(n)

---

## 🔢 6. T(n) = 2T(n / 2) + n

- **Expansão:**
  - T(n) = 2T(n/2) + n  
  - T(n/2) = 2T(n/4) + n/2  
  - T(n/4) = 2T(n/8) + n/4  
  - ...  
  - T(n) = 2^k T(n/2^k) + k×n

- **Parada:**
  - Quando n / 2^k = 1 → 2^k = n → k = log₂(n)

- **Fórmula fechada:**
  - T(n) = n log₂(n) + T(1)  
  - Se T(1) = 1 → T(n) = Θ(n log n)

- **Complexidade:**  
  - Θ(n log n)

---

## 🔢 7. T(n) = 2T(n − 1)

- **Expansão:**
  - T(n) = 2T(n−1)  
  - T(n−1) = 2T(n−2)  
  - T(n−2) = 2T(n−3)  
  - ...  
  - T(n) = 2^k T(n−k)

- **Parada:**
  - Quando n−k = 1 → k = n−1

- **Fórmula fechada:**
  - T(n) = 2^(n−1) × T(1)

- **Complexidade:**  
  - Θ(2ⁿ)

---