# intro-jdbc

> I det här steget gör vi en instans av klassen. Vi slutar alltså använda klass (statiska) metoder.

## App.java

```java
public class App {

  private static final String JDBC_CONNECTION = "jdbc:mysql://localhost:3306/Chinook";
  private static final String JDBC_USER = "iths";
  private static final String JDBC_PASSWORD = "iths";
  private static final String SQL_SELECT_ALL_ARTISTS = "SELECT ArtistId, Name FROM Artist";
  private static final String SQL_COL_ARTIST_ID = "ArtistId";
  private static final String SQL_COL_ARTIST_NAME = "name";
  
  public static void main(String[] args)  {
      App app = new App();
      app.load();
  }

  private void load(){
    Collection<Artist> artists = loadArtists();
    for(Artist artist: artists){
      System.out.println(artist);
    }
  }
  private Collection<Artist>  loadArtists() {
    Collection<Artist> artists = new ArrayList<>();
    Connection con = null;
    ResultSet rs = null;
    try {
      con = con = DriverManager.getConnection(JDBC_CONNECTION, JDBC_USER, JDBC_PASSWORD);
      rs = con.createStatement().executeQuery(SQL_SELECT_ALL_ARTISTS);
      while(rs.next()){
        long id = rs.getLong(SQL_COL_ARTIST_ID);
        String name = rs.getString(SQL_COL_ARTIST_NAME);
        artists.add(new Artist(id, name));
      }
    } catch (SQLException e) {
      System.err.println(String.format("Fel vid läsning av databas %s", e.toString()));
    } finally {
      try {
        rs.close();
      }catch (Exception ignore){}
      try {
        con.close();
      }catch (Exception ignore){}
    }
    return artists;
  }
}
```
