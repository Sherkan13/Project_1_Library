import java.util.*;

public class Biblioteca{
    private final List<Carte> carti = new ArrayList<>();
    Scanner scanner = new Scanner(System.in);

    private Biblioteca(){
        System.out.println("Am deschis Biblioteca!");
    }
    
    private static Biblioteca singleton;
    
    public static Biblioteca getInstance(){
        if (singleton == null){
            singleton = new Biblioteca();
        }
        return singleton;
    }
    
    public void adaugaRoman (String titlu, String autor, int nrPag, String tip){
        carti.add(new Romane(titlu, autor, nrPag, tip));
        System.out.println("Romanul a fost adaugat!");
    }
    
    public void adaugaPoezie (String titlu, String autor, int nrPag, int nrPoezii){
        carti.add(new Poezii(titlu, autor, nrPag, nrPoezii));
        System.out.println("Cartea de poezii a fost adaugata!");
    }
    
    public void afiseazaCarti(){
        for(Carte carte : carti){
            System.out.println(carte);
        }
    }
    
    public void afiseazaCartiSortat(){
        carti.sort(Comparator.comparing(carte -> carte.titlu));
        afiseazaCarti();
    }
    
    public void afiseazaAutor (String titluCarte) throws CarteInexistentaException{
        boolean exista = false;
        for(Carte carte : carti){
            if(carte.titlu.equals(titluCarte)){
                System.out.println(titluCarte + " a fost scrisa de catre " + carte.autor);
                exista = true;
                break;
            }
        }
        if(exista == false){
            throw new CarteInexistentaException(titluCarte + " nu se afla in biblioteca!");
        }
    }
    
    public void exit(){
        String input = "carte";
        String[] param;

        while(!input.toUpperCase().equals("EXIT")) {
            System.out.println("ADD_POEZII, ADD_ROMANE, AF_CARTI, AF_CARTI_SORTATE, AF_AUTOR, EXIT");
            input = scanner.nextLine();
            param = input.split(",\\s+"); /* in parantezele de dupa split am incercat si cu (",\\s*")
                                                sau ("[, ]") dar nu inteleg de ce nu merge*/
            try{
                switch(param[0].toUpperCase()){
                    case "ADD_POEZII":
                        if(param.length != 5)
                            throw new IllegalArgumentException("Nu ai introdus titlul, autorul, nr de pagini sau numarul de poezii!");
                        adaugaPoezie(param[1], param[2], Integer.parseInt(param[3]), Integer.parseInt(param[4]));
                        break;
                    case "ADD_ROMANE":
                        if(param.length != 5)
                            throw new IllegalArgumentException("Nu ai introdus titlul, autorul, nr de pagini sau tipul de roman!");
                        adaugaRoman(param[1], param[2], Integer.parseInt(param[3]), (param[4]));
                        break;
                    case "AF_CARTI":
                        afiseazaCarti();
                        break;
                    case "AF_CARTI_SORTATE":
                        afiseazaCartiSortat();
                        break;
                    case "AF_AUTOR":
                        if(param.length != 2)
                            throw new IllegalArgumentException("Nu ai introdus titlul sau autorul!");
                        afiseazaAutor(param[1]);
                        break;
                    case "EXIT":
                        System.out.println("S-a terminat programul la bilbioteca!");
                        break;
                    default:
                        System.out.println("Eroare!");
                }
            }
            catch(IllegalArgumentException | CarteInexistentaException ex){
                System.out.println(ex.getMessage());
            }
        }
        scanner.close();
    }
}
