## Ambiguedad de rutas

- Cuando escribes dos rutas aun que parezcan distintas en nombre de parametro o tipo de datos, y son por ejemplo
    - 1 path en ambas rutas y ambas son de tipo http get ```@GetMapping``, estas generan ambiguedad y Springboot los toma como la misma ruta. 

### Ejemplo: 
- index y name son 2 path distintons, distintos parametros, pero siguen siendo GetMapping y un solo path, esto genera ambiguedad

   ```java
    @GetMapping("{index}")
    public String showSkillDetail(@PathVariable int index, Model model){....

    @GetMapping("{name}")
    public String showFilterSkill(@PathVariable String name,@PathVariable String level , Model model){

   ```

### Solucion : 

- La mejor forma de solucionar esto es colocar siempre un path descriptivo por cada accion

   ```java
    @GetMapping("/id/{index}")
    public String showSkillDetail(@PathVariable int index, Model model){....

    @GetMapping("/filter/{name}/{level}")
    public String showFilteredSkill(@PathVariable String name,@PathVariable String level , Model model){

    public String showFilterSkill(@PathVariable String name,@PathVariable String level , Model model){...

   ```