<script setup>
import { ref, computed } from "vue";

const acceptedTerms = ref(false);
const showFAQ = ref(false);
const showTerms = ref(false);
const buildingType = ref("");
const width = ref(12); // Default width is 12 feet
const showWhatIsWidth = ref(false);
const depth = ref(12); // Default depth is 12 feet
const showWhatIsDepth = ref(false);
const wallHeight = ref(8); // Default wall height is 8 feet
const showWhatIsWallHeight = ref(false);
const floorArea = computed(() => width.value * depth.value);
const perimeter = computed(() => width.value * 2 + depth.value * 2);
const wallArea = computed(() => perimeter.value * wallHeight.value);
const roofPeakDirection = ref("");
const showWhatIsASkid = ref(false);
const skidType = ref("");
const centerSkid = ref("");
const showWhatIsAJoist = ref(false);
const floorJoistSize = ref("");
const show2x4FloorJoistInfo = ref(false);
const show2x6FloorJoistInfo = ref(false);
const floorPlywoodThickness = ref("2"); // Default floor plywood thickness should be set to 5/8"
const showWhatIsAStud = ref(false);
const wallStudSize = ref("");
const showMoreAbout2x4Studs = ref(false);
const showMoreAbout2x6Studs = ref(false);
const showMoreAboutRoofPeak = ref(false);
const showWhatIsARoofRafter = ref(false);
const showMoreAbout2x4RoofRafters = ref(false);
const showMoreAbout2x6RoofRafters = ref(false);
const roofRafterSize = ref("");
const showWhatIsRoofPitch = ref(false);
const roofPitch = ref("6"); // Default roof pitch should be set to 6:12
const showWhatAreEaves = ref(false);
const eavesOverHang = ref(6); // Default eaves overhang is 6 inches
const showWhatAreGableEnds = ref(false);

function selectBuildingType(type) {
  buildingType.value = type;

  if (type === "animalHousing") {
    width.value = 4;
    depth.value = 4;
    wallHeight.value = 4;
    skidType.value = "2x4flat";
    floorJoistSize.value = "2x4";
    floorPlywoodThickness.value = "1";
    wallStudSize.value = "2x4";
    roofPeakDirection.value = "depth";
    roofRafterSize.value = "2x4";
    roofPitch.value = "6";
    eavesOverHang.value = 6;
  }
}

function loadTheMechanism() {
  const config = {
    width: Number(width.value),
    depth: Number(depth.value),
    wallHeight: Number(wallHeight.value),
    skidType: skidType.value,
    centerSkid: centerSkid.value,
    floorJoistSize: floorJoistSize.value,
    floorPlywoodThickness: floorPlywoodThickness.value,
    wallStudSize: wallStudSize.value,
    roofPeakDirection: roofPeakDirection.value,
    roofRafterSize: roofRafterSize.value,
    roofPitch: roofPitch.value,
    eavesOverHang: eavesOverHang.value,
  };

  const json = JSON.stringify(config, null, 2);

  const blob = new Blob([json], {
    type: "application/json",
  });

  const url = URL.createObjectURL(blob);

  const link = document.createElement("a");

  link.href = url;
  link.download = "config.json";

  link.click();

  URL.revokeObjectURL(url);
}
</script>

<template>
  <div v-if="!acceptedTerms">
    <h1>Welcome to BeaverBuilt Canada</h1>
    <h2>Custom Building Configurator</h2>

    <p>
      Design your own shed, workshop, garage, animal shelter, or outbuilding
      using standard Canadian dimensional lumber and generate a customized set
      of downloadable construction plans and cut lists based on your selections.
    </p>
    <p>
      Designs are generated using North American building conventions, including
      imperial measurements and standard dimensional lumber sizing (for example,
      a nominal 2×4 is treated as 1½" × 3½" actual size. Likewise a nominal 2×6
      is treated as 1½" × 5½" actual size).
    </p>
    <h3>Before You Begin:</h3>
    <ul>
      <li>
        These are emailed or downloadable plans and cut lists intended for
        personal projects, not free downloads.
      </li>
      <li>
        Plans are generated from the selections you make within this
        configurator.
      </li>
      <li>
        No lumber, hardware, building kits, or construction services are
        included.
      </li>
      <li>
        You are responsible for verifying local building codes, permit
        requirements, zoning restrictions, setbacks, site conditions, snow
        loads, wind loads, and all other local requirements before construction.
      </li>
    </ul>
    <div>
      <p>
        <button @click="showFAQ = !showFAQ">Frequently Asked Questions</button>
      </p>
      <div v-if="showFAQ">
        <p>
          <strong>Q: Is this configurator difficult or technical to use</strong
          ><br />
          A: No, this configurator is designed to be simple and user-friendly,
          for all level skilled builders, yet it is robust for advanced
          builders. It has
          <button @click="showMoreInfoExample = !showMoreInfoExample">
            More Info
          </button>
          and
          <button @click="showHelpExample = !showHelpExample">Help</button>
          along the way, should you prefer to see them.
        </p>
        <P
          ><strong
            >Q:What kind of building material can I choose using this
            configurator?</strong
          ><br />
          A: This configurator uses 2x4 and 2x6 standard dimensional lumber.
          Also 4x4 in some cases.
        </P>
        <P
          ><strong
            >Q:What are some of the more advanced features using this
            configurator, that an experienced builder might find useful?</strong
          ><br />
          A: Some of the more advanced features include the ability to choose
          different sizes of lumber for floor joists, wall studs, and roof
          rafters. You can also choose to use a center skid for larger
          structures, and you can specify the roof pitch and peak direction.
          Additionally, you can customize the dimensions of your building to fit
          your specific needs. You can choose rough opening sizes for windows
          and doors, what size of headers you want to use and you can also place
          the openings by stud index and height from floor.
        </P>
        <p>
          <strong>Q: Are these plans free?</strong><br />
          A: No, these are emailed or downloadable plans and cut lists for
          personal projects.
        </p>
        <p>
          <strong>Q: How much do the plans cost?</strong><br />
          A: The cost of the plans is $20. CAD for a single building design and
          cut list. This is a one-time fee for the plans and cut list for your
          specific building design. You will receive a link to download the
          plans and cut list after payment is received. You will also receive a
          link to download the plans and cut list in your email.
        </p>
        <p>
          <strong>Q: What is the early builders club?</strong><br />
          A: The early builders club is a group of early adopters who receive
          free plans and cut lists for testing and feedback. They have also
          agreed to allow Beaver Built Canada to use their projects, pictures
          and their feedback on the BeaverBuiltCanada.ca website and for
          marketing purposes.
        </p>
        <p>
          These are real builders of designs they made themselves using this
          configurator; plans and cut lists. You can see their testimonials by
          clicking this link:
        </p>
        <p>
          Should you wish to join the early builders club, please contact us at:
          (if the link is active, we are looking for early builders to test and
          provide feedback on this configurator) and tell them, "Beaver sent
          me".
        </p>
        <p>
          <strong>Q: Do you provide lumber or hardware?</strong><br />
          A: No, this configurator generates plans and cut lists only. You will
          need to source your own materials.
        </p>
        <p>
          <strong>Q: Can I use these plans for commercial purposes?</strong
          ><br />
          A: No, these plans are intended for personal use only. Commercial use
          is not recommended.
        </p>
      </div>
    </div>
    <p>
      <button @click="showTerms = !showTerms">View Full Disclaimer</button>
    </p>
    <div v-if="showTerms">
      <p>
        BeaverBuilt Canada provides downloadable DIY building plans, drawings,
        and cut lists for informational purposes only.
      </p>
      <p>
        The plans provided are intended as general building guidance and may not
        be suitable for all locations, conditions, climates, or intended uses.
      </p>
      <p>
        The builder is solely responsible for verifying compliance with all
        applicable building codes, permit requirements, zoning regulations,
        property setbacks, utility easements, environmental restrictions, snow
        loads, wind loads, and site-specific conditions before construction
        begins.
      </p>
      <p>
        BeaverBuilt Canada does not provide engineering certification, permit
        approval, code review, site inspection, construction supervision, or
        professional engineering services.
      </p>
      <p>
        All plans are provided as-is without warranties or guarantees of fitness
        for a particular purpose.
      </p>
      <p>
        BeaverBuilt Canada shall not be liable for construction errors,
        omissions, material costs, project delays, code-compliance issues,
        permit denials, property damage, personal injury, or any direct or
        indirect losses arising from the use of these plans.
      </p>
      <p>
        By using this configurator, downloading plans, or constructing from
        those plans, the builder accepts full responsibility for the resulting
        project.
      </p>
    </div>
    <p>
      By continuing, you acknowledge that you have read and understood the
      information above and accept the full disclaimer.
    </p>

    <input type="checkbox" v-model="acceptedTerms" />

    <label> I understand and accept these terms. </label>
  </div>
  <div v-if="acceptedTerms">
    <h1>WELCOME TO:</h1>
    <h1>Beaver Built Canada Shed Configurator</h1>
    <p>
      We have designed this configurator to be simple and user-friendly for
      builders of all skill levels. Whether this is your first building project
      or you've spent years working in the trades, the goal is to make designing
      your building straightforward and enjoyable. For experienced builders,
      additional options and advanced features are available to provide greater
      control over the final design.
    </p>

    <p>
      Visit
      <a
        href="https://beaverbuiltcanada.github.io/memories/trial-chair/"
        target="_blank"
        rel="noopener"
        >BeaverBuiltCanada on github</a
      >
      to read the documentation and configurator help topics (some documentation
      may not be available as work in progress, please check back soon). If you
      require additional help, feel free to contact us by email at:
    </p>
    <div v-if="!buildingType">
      <h2>What Would You Like To Build?</h2>
      <div class="building-card" @click="selectBuildingType('animalHousing')">
        <h3>🐕 Animal & Pet Housing</h3>
        <p>
          Dog houses, chicken coops, shelters and other small outdoor
          structures.
        </p>
      </div>

      <div class="building-card" @click="buildingType = 'yardShed'">
        <h3>🏡 Yard Shed</h3>
        <p>
          Storage sheds, garden sheds, utility buildings, and general backyard
          storage projects.
        </p>
      </div>

      <div class="building-card" @click="buildingType = 'workshopGarage'">
        <h3>🔨 Workshop & Garage</h3>
        <p>Workshops, garages, hobby spaces, and larger utility structures.</p>
      </div>

      <div class="building-card" @click="buildingType = 'customStructure'">
        <h3>⚙️ Custom Structure</h3>
        <p>Start with a blank configuration and build exactly what you need.</p>
      </div>
    </div>

    <div v-else>
      <a href="#" @click.prevent="buildingType = ''">
        ← Choose a different building type
      </a>
      <div v-if="buildingType === 'animalHousing'">
        <div class="building-card">
          <h3>🐕 Animal & Pet Housing</h3>
          <p>
            Dog houses, chicken coops, shelters and other small outdoor
            structures.
          </p>
        </div>
      </div>
      <div v-if="buildingType === 'yardShed'">
        <div class="building-card">
          <h3>🏡 Yard Shed</h3>
          <p>
            Storage sheds, garden sheds, utility buildings, and general backyard
            storage projects.
          </p>
        </div>
      </div>
      <div v-if="buildingType === 'workshopGarage'">
        <div class="building-card">
          <h3>🔨 Workshop & Garage</h3>
          <p>
            Workshops, garages, hobby spaces, and larger utility structures.
          </p>
        </div>
      </div>
      <div v-if="buildingType === 'customStructure'">
        <div class="building-card">
          <h3>⚙️ Custom Structure</h3>
          <p>
            Start with a blank configuration and build exactly what you need.
          </p>
        </div>
      </div>
    </div>

    <h2>Building Dimensions</h2>
    <div class="building-card">
      <p>
        Enter the desired dimensions for your building. The floor area,
        perimeter, and wall area will be calculated and displayed below, for
        your reference.
      </p>

      <div class="dimension-row">
        <label class="dimension-label">Width:</label>
        <select v-model="width">
          <option value="4">4'</option>
          <option value="5">5'</option>
          <option value="6">6'</option>
          <option value="7">7'</option>
          <option value="8">8'</option>
        </select>
        <span class="dimension-value"
          >Your chosen width is {{ width }} feet.</span
        >
      </div>
      <p>
        <button @click="showWhatIsWidth = !showWhatIsWidth">
          What Is Width?
        </button>
      </p>
      <div v-if="showWhatIsWidth">
        <p>
          <strong> Q: What is width?</strong><br />
          A: Width is the side-to-side measurement of the building.
        </p>
        <p>
          <strong>Q: What are we considering to be the width?</strong><br />
          A: Imagine you're standing at the front door facing the building. The
          width runs from left to right across the front wall.
        </p>
      </div>

      <div class="dimension-row">
        <label class="dimension-label">Depth:</label>
        <select v-model="depth">
          <option value="4">4'</option>
          <option value="5">5'</option>
          <option value="6">6'</option>
          <option value="7">7'</option>
          <option value="8">8'</option>
        </select>
        <span class="dimension-value"
          >Your chosen depth is {{ depth }} feet.</span
        >
      </div>
      <p>
        <button @click="showWhatIsDepth = !showWhatIsDepth">
          What Is Depth?
        </button>
      </p>
      <div v-if="showWhatIsDepth">
        <p>
          <strong> Q: What is depth?</strong><br />
          A: Depth is the front-to-back measurement of the building.
        </p>
        <p>
          <strong>Q: What do we mean by the building's depth?</strong><br />
          A: Imagine you're standing at the front door looking inside. The depth
          is the distance your line of sight travels from the front wall to the
          back wall.
        </p>
      </div>

      <div class="dimension-row">
        <label class="dimension-label">Wall Height:</label>
        <select v-model="wallHeight">
          <option value="4">4'</option>
          <option value="5">5'</option>
          <option value="6">6'</option>
          <option value="7">7'</option>
          <option value="8">8'</option>
        </select>
        <span class="dimension-value"
          >Your chosen wall height is {{ wallHeight }} feet.</span
        >
      </div>
      <p>
        <button @click="showWhatIsWallHeight = !showWhatIsWallHeight">
          What Is Wall Height?
        </button>
      </p>
      <div v-if="showWhatIsWallHeight">
        <p>
          <strong> Q: What is wall height?</strong><br />
          A: Wall height is the vertical distance from the floor to the top of
          the building's walls.
        </p>

        <p>
          <strong>Q: How do we determine the wall height?</strong><br />
          A: Wall height is measured from the top of the floor to the top of the
          wall where the roof structure begins. It does not include the gable,
          which is the triangular section of wall above the main wall on a gable
          roof.
        </p>
      </div>

      <p>reference, Floor Area: {{ floorArea }} sq ft</p>
      <p>reference, Perimeter: {{ perimeter }} ft</p>
      <p>reference, Wall Area: {{ wallArea }} sq ft</p>
    </div>

    <h2>Foundation Skids</h2>
    <p>
      <button @click="showWhatIsASkid = !showWhatIsASkid">
        What Is A Skid?
      </button>
    </p>
    <div v-if="showWhatIsASkid">
      <p>
        <strong> Q: What is a skid?</strong><br />
        A: A skid is a horizontal support beam that runs along the bottom of a
        structure, providing stability and support. Skids are typically made of
        pressure-treated lumber because they are usually in contact with the
        ground. They are used in various types of buildings, including sheds,
        workshops, and animal housing. They help distribute the weight of the
        structure evenly and can also make it easier to move the building if
        necessary. By not being permanently fixed to the ground, skids allow for
        some flexibility in placement and can help prevent moisture damage by
        elevating the structure slightly above the ground level. Skids also do
        not require permanent anchoring, which can sometimes be a sticky point
        when navigating building codes and city bylaws.
      </p>
      <p>
        By default, this configurator will use 4 skids around the underside
        perimeter of the building. For larger span buildings it is recommended
        to use a center skid also.
      </p>
    </div>
    <div
      class="building-card"
      :class="{ selected_wood004: skidType === '2x4flat' }"
      @click="skidType = '2x4flat'"
    >
      <h3>Use 2x4 Pressure Treated - Flat Orientation</h3>
      <p>Recommended for smaller structures like animal housing.</p>
    </div>

    <div v-if="skidType">
      <p>Selected Foundation Type; Pressure Treated: {{ skidType }}</p>
    </div>

    <div
      class="building-card"
      :class="{ selected_wood004: skidType === '4x4' }"
      @click="skidType = '4x4'"
    >
      <h3>Use 4x4 Pressure Treated</h3>
      <p>
        Recommended for larger size structures like sheds and small buildings.
      </p>
    </div>

    <div
      class="building-card"
      :class="{ selected_wood004: centerSkid }"
      @click="centerSkid = centerSkid ? '' : 'Use Center Skid'"
    >
      <h3>Use a Center Skid</h3>
      <p>
        Recommended for larger size structures with a deep span. Such as large
        sheds and large buildings.
      </p>
    </div>

    <div v-if="centerSkid">Selected Center Skid: {{ centerSkid }}</div>

    <h2>Floor Joists</h2>
    <p>
      <button @click="showWhatIsAJoist = !showWhatIsAJoist">
        What is a floor joist?
      </button>
    </p>
    <p v-if="showWhatIsAJoist">
      <strong> Q: What is a floor joist?</strong><br />
      A: A floor joist is a horizontal structural element that supports the
      floor of a building. They are typically made of wood or steel and are used
      to distribute the weight of the floor evenly across the structure.
    </p>

    <div
      class="building-card"
      :class="{ selected_wood034: floorJoistSize === '2x4' }"
      @click="floorJoistSize = '2x4'"
    >
      <h3>Use 2x4 Floor Joists</h3>
      <p>
        <button @click="show2x4FloorJoistInfo = !show2x4FloorJoistInfo">
          more about 2x4 joists
        </button>
      </p>
    </div>
    <div v-if="show2x4FloorJoistInfo">
      <p>
        2x4 joists are recommended for smaller structures like animal housing,
        and small light duty sheds for storage, gardening tools and push type
        lawnmowers.
      </p>
      Conventional framing practice is to use the same size floor joists as used
      for wall studs and roof rafters. However, you may choose an unconventional
      approach by mixing sizes, to suit your design intent.

      <p>
        Using 2x4 joists defaults to conventional framing method of 16" on
        centers. You may choose to over-ride that by using more advanced
        settings later. (this feature is currently non-operational)
      </p>
    </div>

    <div v-if="floorJoistSize">
      Selected Floor Joist Size: {{ floorJoistSize }}
    </div>

    <div
      class="building-card"
      :class="{ selected_wood034: floorJoistSize === '2x6' }"
      @click="floorJoistSize = '2x6'"
    >
      <h3>Use 2x6 Floor Joists</h3>
      <p>
        <button @click="show2x6FloorJoistInfo = !show2x6FloorJoistInfo">
          more about 2x6 joists
        </button>
      </p>
      <div v-if="show2x6FloorJoistInfo">
        <p>
          2x6 joists are recommended for larger size structures like equipment
          sheds and small buildings.
        </p>
        <p>
          Conventional framing practice is to use the same size floor joists as
          used for wall studs and roof rafters. However, you may choose an
          unconventional approach by mixing sizes, to suit your design intent.
        </p>
        <p>
          Using 2x6 floor joists defaults to conventional framing method of 24"
          on centers. You may choose to over-ride that by using more advanced
          settings later.(this feature is currently non-operational)
        </p>
      </div>
    </div>

    <h2>Floor Plywood</h2>
    <div class="building-card selected_osb">
      <label class="dimension-label">Plywood Thickness: </label>
      <select v-model="floorPlywoodThickness">
        <option value="1">1/2"</option>
        <option value="2">5/8"</option>
        <option value="3">3/4"</option>
        <option value="4">1"</option>
      </select>
    </div>

    <h2>Wall Studs</h2>
    <p>
      <button @click="showWhatIsAStud = !showWhatIsAStud">
        What is a wall stud?
      </button>
    </p>
    <div v-if="showWhatIsAStud">
      <strong> Q: What is a wall stud?</strong><br />
      A: A wall stud is a vertical structural element that supports the walls of
      a building. They are typically made of wood or steel and are used to
      distribute the weight of the structure evenly across the walls.
    </div>

    <div class="building-card" @click="wallStudSize = '2x4'">
      <h3>Use 2x4 Wall Studs</h3>
      <p>
        <button @click="showMoreAbout2x4Studs = !showMoreAbout2x4Studs">
          more about 2x4 studs
        </button>
      </p>
    </div>
    <div v-if="showMoreAbout2x4Studs">
      <p>
        2x4 studs are recommended for smaller structures like animal housing,
        and small light duty sheds for storage, gardening tools and push type
        lawnmowers.
      </p>
      Conventional framing practice is to use the same size studs as used for
      floor joists and roof rafters. However, you may choose an unconventional
      approach by mixing sizes.

      <p>
        Using 2x4 studs defaults to conventional 16" on centers. You may choose
        to over-ride that by using more advanced settings later.(this feature is
        currently non-operational)
      </p>
    </div>
    <div v-if="wallStudSize">Selected Wall Stud Size: {{ wallStudSize }}</div>

    <div class="building-card" @click="wallStudSize = '2x6'">
      <h3>Use 2x6 Wall Studs</h3>
      <p>
        <button @click="showMoreAbout2x6Studs = !showMoreAbout2x6Studs">
          more about 2x6 studs
        </button>
      </p>
      <div v-if="showMoreAbout2x6Studs">
        <p>
          2x6 studs are recommended for larger structures like equipment sheds
          and small buildings.
        </p>
        <p>
          Conventional framing practice is to use the same size studs as used
          for floor joists and roof rafters. However, you may choose an
          unconventional approach by mixing sizes.
        </p>
        <p>
          Using 2x6 wall studs defaults to conventional 24" on centers. You may
          choose to over-ride that by using more advanced settings later.(this
          feature is currently disabled)
        </p>
      </div>
    </div>
    <h2>Roof Peak Direction</h2>
    <p>
      <button @click="showMoreAboutRoofPeak = !showMoreAboutRoofPeak">
        more about roof peaks and gables
      </button>
    </p>
    <p v-if="showMoreAboutRoofPeak">
      <strong> Q: What is a roof peak?</strong><br />
      A: A roof peak is the highest point of a roof, typically found at the
      center of a gable roof.
    </p>
    <p>
      <strong> Q: What is a gable?</strong><br />
      A: A gable is the triangular section of a wall between the edges of a
      sloping roof.
    </p>

    <div class="building-card" @click="roofPeakDirection = 'width'">
      <h3>Peak Runs Width</h3>
      <p>Gable ends on the left and right walls.</p>
    </div>

    <p v-if="roofPeakDirection">
      Selected Roof Direction: {{ roofPeakDirection }}
    </p>

    <div class="building-card" @click="roofPeakDirection = 'depth'">
      <h3>Peak Runs Depth</h3>
      <p>Gable ends on the front and rear walls.</p>
    </div>
    <h2>Roof Rafters</h2>
    <p>
      <button @click="showWhatIsARoofRafter = !showWhatIsARoofRafter">
        What is a roof rafter?
      </button>
    </p>
    <p v-if="showWhatIsARoofRafter">
      <strong> Q: What is a roof rafter?</strong><br />
      A: A roof rafter is a structural component that supports the roof and
      transfers the load to the walls.
    </p>

    <div class="building-card" @click="roofRafterSize = '2x4'">
      <h3>Use 2x4 Roof Rafters</h3>
      <p>
        <button
          @click="showMoreAbout2x4RoofRafters = !showMoreAbout2x4RoofRafters"
        >
          More about 2x4 roof rafters
        </button>
      </p>
      <div v-if="showMoreAbout2x4RoofRafters">
        <p>
          2x4 roof rafters are typically used in smaller structures and are less
          expensive than larger rafters.
        </p>
        <p>
          2x4 roof rafters are recommended for smaller structures like animal
          housing, and small light duty sheds for storage, gardening tools and
          push type lawnmowers.
        </p>

        <p>
          Conventional framing practice is to use the same size roof rafters as
          used for floor joists and wall studs. However, you may choose an
          unconventional approach by mixing sizes.
        </p>
        <p>
          Using 2x4 roof rafters defaults to conventional 16" on centers. You
          may choose to over-ride that by using more advanced settings
          later.(this feature is currently disabled)
        </p>
      </div>
    </div>

    <p v-if="roofRafterSize">Selected Roof Rafter Size: {{ roofRafterSize }}</p>

    <div class="building-card" @click="roofRafterSize = '2x6'">
      <h3>Use 2x6 Roof Rafters</h3>
      <p>
        <button
          @click="showMoreAbout2x6RoofRafters = !showMoreAbout2x6RoofRafters"
        >
          More about 2x6 roof rafters
        </button>
      </p>
      <div v-if="showMoreAbout2x6RoofRafters">
        <p>
          2x6 roof rafters are recommended for larger structures such as
          equipment sheds, workshops, garages, and small buildings.
        </p>

        <p>
          Compared to 2x4 rafters, 2x6 rafters provide greater strength and are
          better suited for longer spans and heavier roof loads.
        </p>

        <p>
          Using 2x6 roof rafters defaults to conventional 24" on-center spacing.
          This can be customized later using advanced settings. (This feature is
          currently disabled.)
        </p>
      </div>
    </div>

    <h2>Roof Pitch</h2>
    <p>
      <button @click="showWhatIsRoofPitch = !showWhatIsRoofPitch">
        More about roof pitch
      </button>
    </p>
    <div v-if="showWhatIsRoofPitch">
      <p>
        <strong> Q: What is roof pitch?</strong><br />
        A: Roof pitch is the slope of the roof, typically expressed as a ratio
        of rise over run.
      </p>
      <p>
        <strong> Q: What is rise?</strong><br />
        A: Rise is the vertical distance the roof rises over a horizontal
        distance (run).
      </p>
      <p>
        The smaller the ratio, the flatter the roof. A 3:12 ratio is 15 degrees,
        a 6:12 ratio is 26.57 degrees, and a 12:12 ratio is 45 degrees.
      </p>
    </div>
    <div class="building-card">
      <label class="dimension-label">Roof Pitch: </label>
      <input type="number" v-model="roofPitch" min="3" max="12" />
      <span class="dimension-value">:12</span>
    </div>

    <h2>Roof Over Hang: Eaves</h2>
    <p>
      <button @click="showWhatAreEaves = !showWhatAreEaves">
        What are eaves?
      </button>
    </p>
    <div v-if="showWhatAreEaves">
      <p>
        <strong> Q: What are eaves?</strong><br />
        A: Eaves are the portions of the roof that extend past the exterior
        walls of the building. They help direct rain and snow away from the
        siding, doors, windows and walls and can provide additional shade and
        weather protection.
      </p>
    </div>
    <div class="building-card">
      <label class="dimension-label">Eaves Over Hang: </label>
      <input type="number" v-model="eavesOverHang" min="2" max="20" step="2" />
      <span class="dimension-value">inches</span>
    </div>

    <h2>Roof Over Hang: Gable Ends</h2>
    <p>
      <button @click="showWhatAreGableEnds = !showWhatAreGableEnds">
        What are gable ends?
      </button>
    </p>
    <div v-if="showWhatAreGableEnds">
      <p>
        <strong> Q: What are gable ends overhangs?</strong><br />
        A: Gable overhangs are the parts of the roof that extend beyond the
        gable-end walls of the building. They provide additional weather
        protection and can improve the overall appearance of the structure.
      </p>
      <p>
        We are currently working on gable ends overhangs and hope to have that
        option available soon.
      </p>
    </div>
    <div class="building-card">
      <label class="dimension-label">Gable Ends Over Hang: </label>
      <input
        type="number"
        v-model="gableEndsOverHang"
        min="0"
        max="0"
        step="2"
      />
      <span class="dimension-value">inches</span>
    </div>

    <div>
      <button @click="loadTheMechanism">Load The Mechanism</button>
    </div>
  </div>
</template>

<style scoped>
.building-card {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  margin-top: 12px;
}

.dimension-row {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}

.dimension-label {
  width: 100px;
}

.dimension-row input {
  width: 50px;
  text-align: center;
}

.dimension-value {
  width: 250px;
  margin-left: 10px;
}

.selected_wood004 {
  background-image: url("./assets/Wood004_4K-JPG_Color.jpg");
  background-size: cover;
  background-position: center;
  border: 3px solid #d8b36a;
  color: white;
}

.selected_wood034 {
  background-image: url("./assets/Wood034_4K-JPG_Color.jpg");
  background-size: cover;
  background-position: center;
  border: 3px solid #d8b36a;
  color: white;
}

.selected_osb {
  background-image: url("./assets/Chipboard004_4k-JPG_Color.jpg");
  background-size: cover;
  background-position: center;
  border: 3px solid #d8b36a;
  color: rgb(43, 4, 4);
}
</style>
