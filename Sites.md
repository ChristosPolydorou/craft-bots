# Sites
Sites are stationary entities that exist at nodes. They are created by actors at no cost but require resources and effort from the actors to construct. Actors can only put an amount of effort into constructing a site proportional to the number of resources deposited in the site to the total number of resources needed to build that site. 

Once the full amount of resources and effort have been put into a site, it is considered completed and turns into a building. 

## Fields
Site entities have 6 fields that an agent can access to learn about the site (7 if Sites are considered Partially Observable)

* `node`
  * The ID of the node the site is at
* `colour`
  * The colour of the Site (and the type of building it will create) (0 - red, 1 - blue, 2 - orange, 3 - black, 4 - green, 5 - purple)
* `deposited_resources`
  * A list of resources that have been deposited so far in the following format: [# of red resources, # of blue resources, # of orange resources, # of black resources, # of green resources]
* `needed_resources`
  * A list of the resources needed to complete the site in the following format: [# of red resources, # of blue resources, # of orange resources, # of black resources, # of green resources]
* `progress`
  * The amount of progress the site has towards being completed. It is increased by the actors constructing at the site and is limited by the number of deposited resources. The amount of progress depends on the number of resources needed to complete the building and the initialisation files.
* `id`
  * This is the unique, positive, non-zero integer that represents the building in the simulation.
* `observers`*
  * This is a list of the `id`'s of the different actors that can see the site.
  * _*This field is only available if sites are flagged as Partially Observable in the initialisation files (under rules)._