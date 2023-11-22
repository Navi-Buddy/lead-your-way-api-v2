# Cyclescape
Proyecto de Aplicaciones Móviles
---

package com.upc.cyclescape.config;

    import lombok.RequiredArgsConstructor;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.security.authentication.AuthenticationProvider;
    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.http.SessionCreationPolicy;
    import org.springframework.security.web.SecurityFilterChain;
    import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;


    @Configuration
    @EnableWebSecurity
    @RequiredArgsConstructor
    public class SecurityConfig {
        private final JwtAuthenticationFilter jwtAuthFilter;
        private final AuthenticationProvider authenticationProvider;

        @Bean
        public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
            http.cors().and()
                    .authorizeHttpRequests(authorize -> authorize
                            .requestMatchers("/api/cyclescape/v1/auth/**").permitAll()
                      .requestMatchers("/swagger-ui.html")
                      .permitAll()
                      .requestMatchers("/swagger-ui/*", "/v3/api-docs/*")
                      .permitAll()
                            .requestMatchers("/api/cyclescape/v1/users",
                                    "/api/cyclescape/v1/rents","/api/cyclescape/v1/cards","/api/leadyourway/v1/bicycles").authenticated()
                            .anyRequest().authenticated())
                    .csrf(csrf -> csrf.disable())
                    .sessionManagement(session -> session
                            // cuando se establece en STATELESS, significa que no se creará ni
                            // mantendrá ninguna sesión HTTP en el servidor.
                            .sessionCreationPolicy(SessionCreationPolicy.STATELESS))
                    .authenticationProvider(authenticationProvider)
                    .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);

            return http.build();
        }



    }


    --

    en el application.properties esto:
spring.datasource.url=jdbc:mysql://localhost:3306/db_leadyourway?useSSL=false&serverTimeZone=UTC&useLegacyDateTimeCode=false
spring.datasource.username=root
spring.datasource.password=password


#spring.datasource.url=jdbc:mysql://localhost:3306/db_leadyourway?useSSL=false&serverTimezone=UTC
#spring.datasource.username=root
#spring.datasource.password=12345678


spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
spring.jpa.show-sql=true

springdoc.swagger-ui.path=/swagger-ui.html

application.security.jwt.secret-key=3777217A25432A462D4A614E645267556B58703273357538782F413F4428472B
application.security.jwt.expiration=86400000
application.security.jwt.refresh-token.expiration=604800000

---
