# 📘 Démonstration de la régression linéaire par minimisation

## 1. Cas simple : une variable explicative

### Modèle
On a des observations $(x_i, y_i)$ pour $i=1,\dots,n$.  
On cherche une droite :

$$
\hat{y}_i = \beta_0 + \beta_1 x_i
$$

où $\beta_0$ est l’intercept et $\beta_1$ la pente.

---

### Fonction objectif
On minimise la somme des erreurs au carré (moindres carrés ordinaires, OLS) :

$$
L(\beta_0,\beta_1) = \sum_{i=1}^n \big(y_i - (\beta_0 + \beta_1 x_i)\big)^2
$$

---

### Dérivation par rapport à $\beta_0$
$$
\frac{\partial L}{\partial \beta_0} 
= -2 \sum_{i=1}^n \big(y_i - \beta_0 - \beta_1 x_i\big)
$$

Condition du minimum :

$$
\sum_{i=1}^n y_i = n\beta_0 + \beta_1 \sum_{i=1}^n x_i \quad (1)
$$

---

### Dérivation par rapport à $\beta_1$
$$
\frac{\partial L}{\partial \beta_1} 
= -2 \sum_{i=1}^n x_i \big(y_i - \beta_0 - \beta_1 x_i\big)
$$

Condition du minimum :

$$
\sum_{i=1}^n x_i y_i = \beta_0 \sum_{i=1}^n x_i + \beta_1 \sum_{i=1}^n x_i^2 \quad (2)
$$

---

### Système des équations normales
On a donc :

$$
\begin{cases}
\sum y_i = n \beta_0 + \beta_1 \sum x_i \\
\sum x_i y_i = \beta_0 \sum x_i + \beta_1 \sum x_i^2
\end{cases}
$$

---

### Résolution
De (1) :

$$
\beta_0 = \bar{y} - \beta_1 \bar{x}, 
\quad \text{où } \bar{x} = \tfrac{1}{n}\sum x_i, \ \bar{y} = \tfrac{1}{n}\sum y_i
$$

On injecte dans (2) :

$$
\sum x_i y_i - n\bar{x}\bar{y} = \beta_1 \Big( \sum x_i^2 - n\bar{x}^2 \Big)
$$

---

### Numérateur et dénominateur

- Numérateur :  
$$
\sum x_i y_i - n\bar{x}\bar{y} = \sum (x_i - \bar{x})(y_i - \bar{y})
$$

- Dénominateur :  
$$
\sum x_i^2 - n\bar{x}^2 = \sum (x_i - \bar{x})^2
$$

---

### Formule finale
$$
\hat{\beta}_1 = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sum (x_i - \bar{x})^2}
= \frac{\text{Cov}(X,Y)}{\text{Var}(X)}
$$

$$
\hat{\beta}_0 = \bar{y} - \hat{\beta}_1 \bar{x}
$$

---

## 2. Cas matriciel : plusieurs variables explicatives

### Modèle
On écrit le modèle sous forme vectorielle :

$$
y = X\beta + \varepsilon
$$

- $y$ est un vecteur $(n \times 1)$ des observations :  
$$
y = \begin{bmatrix} y_1 \\ y_2 \\ \vdots \\ y_n \end{bmatrix}
$$  

- $X$ est une matrice $(n \times p)$ des variables explicatives (dont une colonne de $1$ pour l’intercept) :  
$$
X = \begin{bmatrix} 
1 & x_{11} & x_{12} & \dots & x_{1,p-1} \\
1 & x_{21} & x_{22} & \dots & x_{2,p-1} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
1 & x_{n1} & x_{n2} & \dots & x_{n,p-1}
\end{bmatrix}
$$

- $\beta$ est un vecteur $(p \times 1)$ des coefficients inconnus :  
$$
\beta = \begin{bmatrix} \beta_0 \\ \beta_1 \\ \vdots \\ \beta_{p-1} \end{bmatrix}
$$

- $\varepsilon$ est le vecteur des erreurs.

---

### Fonction objectif
On veut minimiser la somme des erreurs au carré :

$$
L(\beta) = (y - X\beta)^\top (y - X\beta)
$$

---

### Développement
$$
L(\beta) = y^\top y - 2\beta^\top X^\top y + \beta^\top X^\top X \beta
$$

---

### Gradient
On dérive par rapport à $\beta$ :

$$
\nabla_\beta L = -2 X^\top y + 2 X^\top X \beta
$$

---

### Condition du minimum
On impose $\nabla_\beta L = 0$ :

$$
X^\top X \hat{\beta} = X^\top y
$$

C’est le **système des équations normales** en régression multiple.

---

### Solution
Si $X^\top X$ est inversible :

$$
\hat{\beta} = (X^\top X)^{-1} X^\top y
$$

---

### Interprétation
- $(X^\top X)$ joue le rôle de “matrice de variance-covariance” des variables explicatives.  
- $(X^\top y)$ est l’équivalent des covariances avec la variable $y$.  
- La solution générale est donc une extension du cas simple :  
$$
\hat{\beta}_j = \frac{\text{Cov}(X_j, Y)}{\text{Var}(X_j)} \quad \text{(après projection sur les autres variables)}
$$

