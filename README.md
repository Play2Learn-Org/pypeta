# Pypeta

Simulate [Liquid Handling Robots](https://en.wikipedia.org/wiki/Liquid_handling_robot) in Python with an interactive web interface.

The goal is to have a platform where:

 * **Developers** can experiment with writing programs for robots, without having to deal with physical errors like destroying equipment.
 * **Scientists** can define experiments and outcomes in a dynamic, reproducable way.
 * **Students** can play around and get a feel for the pipetting work and robot management, as well as the development of software in this space.
 * **Manufacturers** can have a market-wide standard of non-competitive software and API features with reduced and shared development cost.

In an ideal world programs written for this simulator should be usable or compilable to real-world robot interfaces.

## Existing Work

 * a very simplified vision could be this simplified [coffee machine simulator](https://github.com/Play2Learn-Org/coffeemachine)
 * The idea came from a collaboration with [YSEQ](https://github.com/YSEQ-GmbH) to get some old [TECAN robots](https://www.tecan.de/) working again.
   * We found this [library on Github](https://github.com/tolland/python-tecan-genesis) by Tom Hodder.
   * Also this [old Perl project](https://metacpan.org/dist/Robotics) by Jonathan Cline.
 * The [Sculpting Evolution Group at MIT](https://www.media.mit.edu/groups/sculpting-evolution) has created a similar [Python Project](https://github.com/dgretton/pyhamilton) for [Hamilton](https://www.media.mit.edu/groups/sculpting-evolution) robots. ([Paper](https://www.embopress.org/doi/pdf/10.15252/msb.20209942))
 * The [EvoBot](https://www.thingiverse.com/thing:2776125) project designed an open-source hardware robot with APIs. ([Paper](https://www.mdpi.com/2076-3417/10/3/814/htm))
 * The [FINDUS](https://slas-technology.org/article/S2472-6303(22)01025-1/fulltext#pageBody) is a similar 3d printed open source robot project, which might be less well developed.
 * The [OpenLH](https://www.instructables.com/OpenLH/) project focuses on a single pipette. A first review creates the feeling of having an actual community around it. They are using the open source arm [UArm](https://github.com/uArm-developer).
 * [opentrons](https://opentrons.com/) is a company that creates open source equipment for labs.
 * [Aliqbot](http://pipettejockey.com/2017/10/17/the-aliqbot-a-diy-liquid-handling-robot/) seems to be a personal project that attempts something similar to EvoBot and FINDUS, built by Pipette Jockey. There is also a [video](http://pipettejockey.com/2018/01/03/making-a-opentrons-compatible-liquid-handling-robot/), where he built a complete Robot with Opentrons.
