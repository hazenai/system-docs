# Site Rules Specification
A configuration consists of 6 or 7 parameters depending on the requirements which are:
1. Site Name (color coded red in above sample configuration)
2. Constants
3. States
4. Zones
5. ALPR Zones
6. Alias
7. Rules

Sample site configuration rules can be found [here](./site_config.yaml)

## 1. SITE NAME:
Site name corresponds to the site where the service is deployed and is the first thing mentioned
in the configuration.2. Constants:
Constants are the values which are required to be known by violation service beforehand and
refer to values such as frame rate, image height, image width etc. etc. that remain almost the
same in most of the sites.

## 3. States:
States are the factors whose values vary and depending on those values, it is determined
whether an object has committed a violation or not. For example, in the above sample
configuration, for an object to commit a `running red light` violation, the value of the factor
`LIGHT` has to be red.
Different values of a same factor are written in a string, seperated by pipe characters:
`Light:​ "red|yellow|green|black"`
In the above example, Light is a factor and `red`, `yellow`, `green`, and `black` are the possible
values.

## 4. Zones:
Zones are the annotated regions on an image which are used to determine the passage of an
object on an intersection. The figure below shows an annotated frame with zones:
![alt text](Isolated.jpg "Title")

Zones are defined as multipolygons and the corners of that polygon are mentioned in the
configuration.
Following operations can be performed on zones:
* `+` which corresponds to the union of two regions
* `-` which subtracts one zone from another zone
* `*` which gives the intersection of two zones

## 5. ALPR Zones:
ALPR zones are defined if LPR is required in some scenarios.

## 6. Alias:
Alias is a name given to a combination of zones.
`Enter:​ $Lane_1 + $Lane_2 + $Lane_3`
Each zone involved in aliasing is preceded by a `$` sign.

## 7. Rules:
The first thing in a rule is its name followed by a sequence of rulelines. Rule name cannot have
any spaces in it.

`RunningRedLight:

    RL1:
        Zone: $Enter
        Shot: a_shot
        States:
            vehicle_category: '!person'
            Light: ‘red’
    RL2:
        Zone: $
    RL3:
        Zone: $Zebra_crossing
        Shot: b_shot
    States:
        Light: ‘red’
    RL4:
        Zone: $Exit`
        

Here, ‘RunningRedLight’ is the name of the rule. Rulelines are defined later on as `RL1`, `RL2`,
`RL3` and `RL4`. This rule defines a violation RunningRedLight in which an object commits a
violation if it passes from Enter (defined as an alias) to Zebra_Crossing while the light is red and
then passes through the Exit Zone.
RL1 defines the zone as `$Enter` and Shot as `a_shot` and finally states are mentioned. Zones can
either be given as an alias `$Enter` or they could be provided as an algebraic expression such as
`$Lane_1 + $Lane_2 + $Lane_3`. Both cases are acceptable. If a shot is required to be captured at
the end of that zone, then Shot is defined in ruleline with its name which is `a_shot` in the above
case. States in the ruleline provide the state of the environment at that instance which for `Light` is
`red` and for vehicle category is `!person`. States in rulelines support following expressions:

* `!` if a specific value of a factor has to be excluded. Like in the above example, the rule
applies to every vehicle category except a person.
* `|` which performs the **OR** operation. For instance, if it was required that even crossing the
region while the light is yellow is a violation, then State Light could be defined as Light:
‘red|yellow’.

**AND** operator is not supported because the value of states are mutually exclusive, and there
can’t be two values of a single factor at a given time. Rest of the rulelines are defined in the same
way as RL1 but RL2 is a unique ruleline, which is written when the regions between RL1 and RL3are disjoint. In that case a ruleline with Zone $ is defined, which corresponds to any region. So, if
the Enter region and Zebra regions are disjoint, we don’t care where the object goes in between
them as long as it enters the Zebra region after entering the Enter region at some point. Another
addition to the ruleline can be of the Duration parameter such as Duration: 1.5 where 1.5 is the
duration in seconds. A rule employing the parameter Duration has been provided in the above
sample configuration, named as `EnteringRightRegion`.

### Caveats:

1. ALPR zones are not used in rulelines. They are just defined in the beginning of the
configuration and then violation service determines the LPR zone of the object
committing the violation.
2. Speaking of ALPR zones, LPR information would only be available if the ALPR zones are
defined before the region used in the last ruleline of a rule.
3. Composite zones are supported only as aliases or the zone subsection of rulelines.