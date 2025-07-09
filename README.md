# TuneHub---Musicplatform
Full-stack music streaming platform using Java Spring Boot, MySQL, and HTML/CSS/JS.  Let me know if you want a full README.md with features, tech stack, screenshots, and setup instructions.
My Role – Detailed with Explanation & Code

MY ROLES :-
🔐 1. User Authentication
Explanation:
Authenticate users during login using Spring Security to ensure only registered users can access the app.

Code Example (Spring Security Configuration):

java
Copy
Edit
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/register", "/login").permitAll()
                .anyRequest().authenticated()
            .and()
            .formLogin()
            .loginPage("/login")
            .defaultSuccessUrl("/home", true)
            .permitAll();
    }

    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication()
            .withUser("user").password("{noop}password").roles("USER");
    }
}
🎵 2. Uploading Songs and Saving in MySQL
Explanation:
Allow users to upload song files. Metadata like song name, artist, and file path are saved in the database.

Code Example (Controller + Entity):

java
Copy
Edit
@PostMapping("/upload")
public String uploadSong(@RequestParam("file") MultipartFile file, @RequestParam String name) throws IOException {
    Song song = new Song();
    song.setName(name);
    song.setFilePath("songs/" + file.getOriginalFilename());

    // Save file to folder
    file.transferTo(new File("songs/" + file.getOriginalFilename()));

    // Save to DB
    songRepository.save(song);
    return "Song uploaded successfully!";
}
java
Copy
Edit
@Entity
public class Song {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String filePath;
}
🎶 3. Creating & Retrieving Playlists
Explanation:
Users can create playlists and add songs to them. Data is stored in the database with relationships.

Code Example (Playlist + Repository):

java
Copy
Edit
@Entity
public class Playlist {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;

    @ManyToMany
    private List<Song> songs;
}
java
Copy
Edit
@PostMapping("/playlist")
public String createPlaylist(@RequestBody Playlist playlist) {
    playlistRepository.save(playlist);
    return "Playlist created!";
}

@GetMapping("/playlist/{id}")
public Playlist getPlaylist(@PathVariable Long id) {
    return playlistRepository.findById(id).orElse(null);
}
📮 4. API Testing using Postman
Explanation:
Used Postman to test each REST API endpoint (like /upload, /playlist, /login, etc.) to ensure backend is working.

Steps in Postman:

Select method: POST

Enter URL: http://localhost:8080/upload

Go to Body → form-data

Add fields:

file: Select audio file

name: Enter song name

Click Send
