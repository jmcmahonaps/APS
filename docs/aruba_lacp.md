### Notes for Creating Active LACP Port-Channels in ArubaOS

We ran into a vendor interoperability issue between our ArubaOS and Juniper EX4600 switches. Where the port channel configuration for an aruba-to-aruba aggregated link looks like this, essentially:

`interface port-channel "$channel-id"`\
   `description "$description"`\
   `mstp-profile "$mstp-profile"`  \
   `pvst-port-profile "$pvst-profile`  \
   `enet-link-profile "$enet-profile"`  \
   `switching-profile "$switching-profile"`  \
   `port-channel-members $members`
   
When trying to complete the same configuration between an Aruba 3500 and a Juniper EX4600, the aggegate interface (ae) on the Juniper side wouldn't come up, despite the configuration being correct.

After some frustration, the solution was to create the port-channel on the 3500:

`interface port-channel $gid \\ gid being the port-channel interface number`

but instead of adding the desired interfaces to the port-channel, creating a separate LACP profile with a group-id corresponding to the interface number of the port channel.

`interface-profile lacp-profile "$profile_name"`\
   `group-id $gid \\ where $gid corresponds to the port-channel interface number we created earlier`\
   `mode active`\
   `timeout short`

Then, configuring the physical interfaces to have "membership" in the lacp-profile you just created.

`interface gigabitethernet "$interface"`\
   `description "$pc_desc"`\
   `lldp-profile "lldp-factory-initial"`\
   `lacp-profile "$profile-name" \\ where $profile-name corresponds to the lacp profile in the last step`
   
After doing this, the aggregate interface came up as expected. Note we didn't have to ever actually assign the interfaces to the port channel. This configuration makes the interfaces a little more "ethereal" than I'd like but it got the job done.
