# U.S.-Elections
Visualizing Elections Data

#How many total votes did each party get in the years leading up to 2026?

counts <- raw %>% 
  group_by(state, party_detailed, year) %>% 
  summarise(votes = sum(candidatevotes))

#Keep only one state for simplicity

counts_az <- counts %>% 
  filter(state == "ARIZONA") %>%
  filter(party_detailed != "")

#visualize year-over-year growth via gif

gif_az <- counts_az %>%
  ggplot( aes(x=year, y=votes, group=party_detailed, color=party_detailed)) +
  geom_line() +
  geom_point() +
  theme_minimal(base_family = "Arial") +
  scale_color_viridis_d() +
  scale_y_continuous(labels = label_comma()) +
  ggtitle("Vote Count by Party Affilation, Arizona") +
  ylab("Number of votes") +
  transition_reveal(year)

animate(gif_az, renderer = gifski_renderer())
anim_save("animation_az.gif")
