```
const oneBlockSize = 20;
```
oneBlockSize : Définit la taille d'un bloc du labyrinthe, ici chaque cellule du labyrinthe mesurera 20 pixels de côté.

```
function generateMaze(_domElement, _cellSize) {
	const canvas = document.querySelector(_domElement);
	const ctx = canvas.getContext("2d");
	const cellSize = _cellSize;
	const rows = Math.floor((canvas.height - 40) / cellSize);
	const cols = Math.floor(canvas.width / cellSize);
	const maze = new Array(rows).fill(null).map(() => new Array(cols).fill(1));
```
* generateMaze : Fonction principale qui génère un labyrinthe et le dessine dans un élément canvas.
* canvas : Sélectionne l'élément canvas à partir du DOM via le sélecteur _domElement.
* ctx : Contexte de dessin 2D du canvas, utilisé pour dessiner le labyrinthe.
* cellSize : Taille d'une cellule du labyrinthe passée en paramètre.
* rows : Calcule le nombre de lignes du labyrinthe en fonction de la hauteur du canvas et de la taille des cellules.
* cols : Calcule le nombre de colonnes du labyrinthe en fonction de la largeur du canvas.
* maze : Initialise une grille (tableau 2D) représentant le labyrinthe, où chaque cellule est initialement remplie avec le chiffre 1 (représentant un mur).

```
function backtrack(x, y) {
	maze[y][x] = 2;
	const directions = shuffleArray([
		{ dx: 0, dy: -1 }, // Haut
		{ dx: 0, dy: 1 }, // Bas
		{ dx: -1, dy: 0 }, // Gauche
		{ dx: 1, dy: 0 }, // Droite
	]);

	for (const { dx, dy } of directions) {
		const nx = x + dx * 2;
		const ny = y + dy * 2;

		if (nx >= 0 && nx < cols && ny >= 0 && ny < rows && maze[ny][nx] === 1) {
			maze[y + dy][x + dx] = 0;
			backtrack(nx, ny);
		}
	}
}
```
* backtrack : Fonction récursive qui génère le chemin du labyrinthe en suivant un algorithme de backtracking.
  - Marque la cellule actuelle comme un chemin en définissant la valeur 2 (un chemin visité).
  - Mélange les directions (haut, bas, gauche, droite) aléatoirement pour créer des chemins uniques.
  - Pour chaque direction, vérifie si la cellule suivante est valide (à l'intérieur des limites du labyrinthe et un mur).
  - Si une cellule valide est trouvée, elle est marquée comme un chemin (0), et la fonction est appelée de manière récursive pour continuer l'exploration.

```
function shuffleArray(array) {
	for (let i = array.length - 1; i > 0; i--) {
		const j = Math.floor(Math.random() * (i + 1));
		[array[i], array[j]] = [array[j], array[i]];
	}
	return array;
}
```
* shuffleArray : Mélange les éléments d'un tableau (ici, les directions de déplacement) pour que les chemins du labyrinthe soient aléatoires à chaque exécution.

```
backtrack(1, 1);
```

* Démarre l'algorithme de backtracking à partir de la cellule en position (1, 1).
