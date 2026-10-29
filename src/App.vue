<!--  this is a Vue/HTML comment -->
<script setup>
import { ref, computed, watch } from "vue";
import roofPeakWidth from "./assets/roofPeakWidth.png";
import roofPeakDepth from "./assets/roofPeakDepth.png";
import twoIso from "./assets/two_iso.png";
import skidsShowing from "./assets/skidsShowing.png";
import tapeMeasure from "./assets/tapeMeasureBorder.png";

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
const plywoodDisplay = computed(() => {
  switch (floorPlywoodThickness.value) {
    case "1":
      return '1/2"';
    case "2":
      return '5/8"';
    case "3":
      return '3/4"';
    case "4":
      return '1"';
    default:
      return floorPlywoodThickness.value;
  }
});
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
const dimensionWarning = computed(() => {
  return Number(width.value) > Number(depth.value);
});

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

  if (type === "yardShed") {
    width.value = 8;
    depth.value = 8;
    wallHeight.value = 6;
    skidType.value = "2x4flat";
    floorJoistSize.value = "2x4";
    floorPlywoodThickness.value = "2";
    wallStudSize.value = "2x4";
    roofPeakDirection.value = "depth";
    roofRafterSize.value = "2x4";
    roofPitch.value = "8";
    eavesOverHang.value = 8;
  }

  if (type === "workshopGarage") {
    width.value = 12;
    depth.value = 8;
    wallHeight.value = 8;
    skidType.value = "4x4";
    floorJoistSize.value = "2x6";
    floorPlywoodThickness.value = "4";
    wallStudSize.value = "2x6";
    roofPeakDirection.value = "width";
    roofRafterSize.value = "2x6";
    roofPitch.value = "12";
    eavesOverHang.value = 18;
  }

  if (type === "customStructure") {
    width.value = 12;
    depth.value = 12;
    wallHeight.value = 8;
    skidType.value = "4x4";
    floorJoistSize.value = "2x6";
    floorPlywoodThickness.value = "3";
    wallStudSize.value = "2x6";
    roofPeakDirection.value = "depth";
    roofRafterSize.value = "2x6";
    roofPitch.value = "10";
    eavesOverHang.value = 10;
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
    <!-- ====================Landing PAGE 1====WELCOME====FAQ===Terms & Disclaimer Page =========
     ============================================================-->
    <h1>Welcome to BeaverBuiltCanada</h1>
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
    <!--Start of BULLET POINTS on LANDING PAGE-->
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
    <!--Start of FAQ-->
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
          <strong>Q: What is the early builders honor club?</strong><br />
          A: The early builders honor club is a group of early adopters who
          receive free plans and cut lists for testing and feedback. They have
          also agreed to allow Beaver Built Canada to use their projects,
          pictures and their feedback on the BeaverBuiltCanada.ca website and
          for marketing purposes. This is based on the honor system, and their
          feedback is greatly appreciated and helps drive improvements for
          everybody.
        </p>
        <p>
          These are real builders of designs they made themselves using this
          configurator; plans and cut lists. You can see their testimonials by
          clicking this link:
        </p>
        <p>
          Should you wish to join the early builders honor club, please contact
          us at: (if the link is active, we are looking for early builders to
          test and provide feedback on this configurator) and tell them, "Beaver
          sent me".
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
    <!--Start of VIEW FULL DISCLAIMER-->
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
  <!-- End Terms & Disclaimer Page -->
  <!--========================================Start of PAGE 2 LANDING after I accept=========
  ============================================Welcome----Configurator v1.0=============-->
  <div v-if="acceptedTerms">
    <!-- Main Building Type selection for Configurator -->
    <!-- Top of page 2 landing -->
    <h1>WELCOME TO:</h1>
    <h2>Beaver Built Canada: Custom Building Configurator, v1.0</h2>
    <p>and Thank You for deciding to try out our configurator!</p>
    <div v-if="!buildingType" class="sticky-tape">
      <img :src="tapeMeasure" alt="Tape Measure" class="tape_measure_image" />
    </div>
    <p>
      We have designed this configurator to be simple, enjoyable and
      user-friendly for builders of all skill levels. Whether this is your first
      building project or you've spent years working in the trades, the goal is
      to make designing your building straightforward and enjoyable. For
      experienced builders, additional options and advanced features are
      available to provide greater control over the final design.
    </p>
    <!--=====================This is currently a link to a photo for development phase=========-->
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
    <!-- ==========================WHAT WOULD YOU LIKE TO BUILD?============================
     ==============================top of building-type cards of the class selectBuildingType====
     ================================================== -->

    <div v-if="!buildingType">
      <h3 class="my-special-heading">What Would You Like To Build?</h3>
      <div class="building-card" @click="selectBuildingType('animalHousing')">
        <h3>🐕 Animal & Pet Housing</h3>
        <p>
          Dog houses, chicken coops, shelters and other small outdoor
          structures.
        </p>
      </div>

      <div class="building-card" @click="selectBuildingType('yardShed')">
        <h3>🏡 Yard Shed</h3>
        <p>
          Storage sheds, garden sheds, utility buildings, and general backyard
          storage projects.
        </p>
      </div>

      <div class="building-card" @click="selectBuildingType('workshopGarage')">
        <h3>🔨 Workshop & Garage</h3>
        <p>Workshops, garages, hobby spaces, and larger utility structures.</p>
      </div>

      <div class="building-card" @click="selectBuildingType('customStructure')">
        <h3>⚙️ Custom Structure</h3>
        <p>Start with a blank configuration and build exactly what you need.</p>
      </div>
    </div>
    <!-- ==========After Building type selection====Tape measure should get smaller to lhs========================
     =================top of PAGE 3===building-type cards of the "sticky" kind.================================================== -->
    <!--============Choose a different building link===sticks to top============-->
    <div v-else class="selected-building-banner">
      <div v-if="buildingType === 'animalHousing'">
        <div class="building-card">
          <a
            href="#"
            class="change-building-link"
            @click.prevent="buildingType = ''"
            >← Choose a different building type</a
          >
          <h3>🐕 Animal & Pet Housing</h3>
          <p>
            Dog houses, chicken coops, shelters and other small outdoor
            structures.
          </p>
        </div>
      </div>
      <div v-if="buildingType === 'yardShed'">
        <div class="building-card">
          <a
            href="#"
            class="change-building-link"
            @click.prevent="buildingType = ''"
            >← Choose a different building type</a
          >
          <h3>🏡 Yard Shed</h3>
          <p>
            Storage sheds, garden sheds, utility buildings, and general backyard
            storage projects.
          </p>
        </div>
      </div>
      <div v-if="buildingType === 'workshopGarage'">
        <div class="building-card">
          <a
            href="#"
            class="change-building-link"
            @click.prevent="buildingType = ''"
            >← Choose a different building type</a
          >
          <h3>🔨 Workshop & Garage</h3>
          <p>
            Workshops, garages, hobby spaces, and larger utility structures.
          </p>
        </div>
      </div>
      <div v-if="buildingType === 'customStructure'">
        <div class="building-card">
          <a
            href="#"
            class="change-building-link"
            @click.prevent="buildingType = ''"
            >← Choose a different building type</a
          >
          <h3>⚙️ Custom Structure</h3>
          <p>
            Start with a blank configuration and build exactly what you need.
          </p>
        </div>
      </div>
    </div>
    <!-- End of Main Building Type selection=========== -->
    <!--==========This is where 2 column layout should start...me thinks=========-->
    <!-- Start of Building Dimensions above two orthographic views with view cubes -->
    <div v-if="buildingType" class="configurator-layout">
      <aside class="page3-sidebar">
        <div class="page3-sticky-panel">
          <img :src="tapeMeasure" alt="Tape Measure" class="page3-tape-image" />
          <div class="summary-card">
            <p>
              (these are system default values and will update with your
              choices)
            </p>
            <h4>Your Current Building Selections:</h4>
            <div class="summary-row">
              <span>Width:</span><span>{{ width }}'</span>
            </div>
            <div class="summary-row">
              <span>Depth:</span><span>{{ depth }}'</span>
            </div>
            <div class="summary-row">
              <span>Wall Height:</span><span>{{ wallHeight }}'</span>
            </div>
            <div class="summary-row">
              <span>Foundation:</span>
              <span>{{ skidType }}</span>
            </div>
            <div class="summary-row">
              <span>Center Skid:</span>
              <span>{{ centerSkid || "No" }}</span>
            </div>
            <div class="summary-row">
              <span>Floor Joists:</span>
              <span>{{ floorJoistSize }}</span>
            </div>
            <div class="summary-row">
              <span>Plywood:</span>
              <span>{{ plywoodDisplay }}</span>
            </div>
            <div class="summary-row">
              <span>Wall Studs:</span>
              <span>{{ wallStudSize }}</span>
            </div>
            <div class="summary-row">
              <span>Peak:</span>
              <span>{{ roofPeakDirection }}</span>
            </div>
            <div class="summary-row">
              <span>Rafters:</span>
              <span>{{ roofRafterSize }}</span>
            </div>
            <div class="summary-row">
              <span>Pitch:</span>
              <span>{{ roofPitch }}:12</span>
            </div>
            <div class="summary-row">
              <span>Eaves:</span>
              <span>{{ eavesOverHang }}"</span>
            </div>
          </div>
        </div>
      </aside>
      <div class="options-column">
        <div class="main-column">
          <h2>Building Dimensions</h2>
          <!-- double isometric pic with the view cubes -->

          <div class="reference-card">
            <p>
              Enter the desired dimensions for your building. The floor area,
              perimeter, and wall area will be calculated and displayed below,
              for your reference.
            </p>
            <h4>Dimensions Reference</h4>
            <div class="dimension-reference-container">
              <img
                :src="twoIso"
                alt="Building dimensions reference"
                class="dimensions_reference_image"
              />
              <!--Start of RED GREEN BLUE legend-->
              <div class="dimension-legend">
                <div class="legend-width">Width</div>
                <div class="legend-depth">Depth</div>
                <div class="legend-height">Wall Height</div>
              </div>
            </div>
            <!--=================This is the test spot for column set up closing div==============-->
          </div>
          <!--Start of WIDTH drop down-->
          <div class="dimension-row">
            <label class="dimension-label">Width:</label>
            <select v-model="width">
              <option value="4">4'</option>
              <option value="5">5'</option>
              <option value="6">6'</option>
              <option value="7">7'</option>
              <option value="8">8'</option>
              <option value="9">9'</option>
              <option value="10">10'</option>
              <option value="11">11'</option>
              <option value="12">12'</option>
              <option value="13">13'</option>
              <option value="14">14'</option>
              <option value="15">15'</option>
              <option value="16">16'</option>
              <option value="17">17'</option>
              <option value="18">18'</option>
              <option value="19">19'</option>
              <option value="20">20'</option>
              <option value="21">21'</option>
              <option value="22">22'</option>
              <option value="23">23'</option>
              <option value="24">24'</option>
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
              A: Imagine you're standing at the front door facing the building.
              The width runs from left to right across the front wall.
            </p>
          </div>
          <!--Start of DEPTH drop down-->
          <div class="dimension-row">
            <label class="dimension-label">Depth:</label>
            <select v-model="depth">
              <option value="4">4'</option>
              <option value="5">5'</option>
              <option value="6">6'</option>
              <option value="7">7'</option>
              <option value="8">8'</option>
              <option value="9">9'</option>
              <option value="10">10'</option>
              <option value="11">11'</option>
              <option value="12">12'</option>
              <option value="13">13'</option>
              <option value="14">14'</option>
              <option value="15">15'</option>
              <option value="16">16'</option>
              <option value="17">17'</option>
              <option value="18">18'</option>
              <option value="19">19'</option>
              <option value="20">20'</option>
              <option value="21">21'</option>
              <option value="22">22'</option>
              <option value="23">23'</option>
              <option value="24">24'</option>
            </select>
            <span class="dimension-value"
              >Your chosen depth is {{ depth }} feet.</span
            >
            <div v-if="dimensionWarning" class="warning_box">
              ⚠️ Depth is typically treated as the longer dimension of the
              building. Future floor joist, center skid, and roof calculations
              will use this orientation as the default assumption. You may
              continue with any dimensions, but understanding this convention
              will help when selecting later options.
            </div>
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
              A: Imagine you're standing at the front door looking inside. The
              depth is the distance your line of sight travels from the front
              wall to the back wall.
            </p>
          </div>
          <!--===========================================Start of WALL HEIGHT drop down===
        =============================================-->
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
              A: Wall height is the vertical distance from the floor to the top
              of the building's walls.
            </p>

            <p>
              <strong>Q: How do we determine the wall height?</strong><br />
              A: Wall height is measured from the top of the floor to the top of
              the wall where the roof structure begins. It does not include the
              gable, which is the triangular section of wall above the main wall
              on a gable roof.
            </p>
          </div>

          <p>reference, Floor Area: {{ floorArea }} sq ft</p>
          <p>reference, Perimeter: {{ perimeter }} ft</p>
          <p>reference, Wall Area: {{ wallArea }} sq ft</p>
        </div>

        <h2>Foundation Skids</h2>

        <div class="reference-card reference_images">
          <h4>Foundation Skids Reference</h4>
          <img
            :src="skidsShowing"
            alt="Foundation skids reference"
            class="skids_reference_image"
          />
          <p>
            <button @click="showWhatIsASkid = !showWhatIsASkid">
              What Is A Skid?
            </button>
          </p>
        </div>

        <div v-if="showWhatIsASkid">
          <p>
            <strong> Q: What is a skid?</strong><br />
            A: A skid is a horizontal support beam that runs along the bottom of
            a structure, providing stability and support. Skids are typically
            made of pressure-treated lumber because they are usually in contact
            with the ground. They are used in various types of buildings,
            including sheds, workshops, and animal housing. They help distribute
            the weight of the structure evenly and can also make it easier to
            move the building if necessary. By not being permanently fixed to
            the ground, skids allow for some flexibility in placement and can
            help prevent moisture damage by elevating the structure slightly
            above the ground level. Skids also do not require permanent
            anchoring, which can sometimes be an advantage when navigating
            building codes and municipal bylaws. As always, be sure to follow
            the rules and requirements of your local jurisdiction.
          </p>
          <p>
            By default, this configurator will use four skids around the
            underside perimeter of the building. For larger depth span buildings
            it is recommended to use a center skid also. The center skid will be
            placed equal distance and parallel between the front skid and the
            back skid.
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

        <div v-if="skidType" class="current_selection">
          <p>Selected Foundation Skid Type; Pressure Treated: {{ skidType }}</p>
        </div>

        <div
          class="building-card"
          :class="{ selected_wood004: skidType === '4x4' }"
          @click="skidType = '4x4'"
        >
          <h3>Use 4x4 Pressure Treated</h3>
          <p>
            Recommended for larger size structures like sheds and small
            buildings.
          </p>
        </div>

        <div
          class="building-card"
          :class="{
            selected_wood004: centerSkid,
            current_selection: centerSkid,
          }"
          @click="centerSkid = centerSkid ? '' : 'Use Center Skid'"
        >
          <h3>Use a Center Skid</h3>
          <p>
            Recommended for larger size structures with a deep span. Such as
            large sheds and large buildings.
          </p>
        </div>

        <div class="current_selection" v-if="centerSkid">
          Selected Center Skid: {{ centerSkid }}
        </div>
        <!--==============================================================FLOOR JOISTS======
=====================================================================================-->
        <h2>Floor Joists</h2>
        <p>
          <button @click="showWhatIsAJoist = !showWhatIsAJoist">
            What is a floor joist?
          </button>
        </p>
        <div v-if="showWhatIsAJoist">
          <p>
            <strong> Q: What is a floor joist?</strong><br />
            A: A floor joist is a horizontal structural element that supports
            the floor of a building. They are typically made of wood or steel
            and are used to distribute the weight of the floor evenly across the
            structure.
          </p>

          <p>
            By default, this configurator will place the floor joists in a depth
            orientation. If you are designing a rectangular shaped structure, it
            would be better to have the depth be the longer side, in the event
            you choose to use a center skid to support the floor joists in the
            middle of their length.
          </p>
        </div>

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
            2x4 joists are recommended for smaller structures like animal
            housing, and small light duty sheds for storage, gardening tools and
            push type lawnmowers.
          </p>
          Conventional framing practice is to use the same size floor joists as
          used for wall studs and roof rafters. However, you may choose an
          unconventional approach by mixing sizes, to suit your design intent.

          <p>
            Using 2x4 joists defaults to conventional framing method of 16" on
            centers. You may choose to over-ride that by using more advanced
            settings later. (this feature is currently non-operational)
          </p>
        </div>

        <div class="current_selection" v-if="floorJoistSize">
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
              2x6 joists are recommended for larger size structures like
              equipment sheds and small buildings.
            </p>
            <p>
              Conventional framing practice is to use the same size floor joists
              as used for wall studs and roof rafters. However, you may choose
              an unconventional approach by mixing sizes, to suit your design
              intent.
            </p>
            <p>
              Using 2x6 floor joists defaults to conventional framing method of
              24" on centers. You may choose to over-ride that by using more
              advanced settings later.(this feature is currently
              non-operational)
            </p>
          </div>
        </div>
        <!--=========================================================PLYWOOD============
=========================-->
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
          A: A wall stud is a vertical structural element that supports the
          walls of a building. They are typically made of wood or steel and are
          used to distribute the weight of the structure evenly across the
          walls.
        </div>

        <div
          class="building-card"
          :class="{ selected_wood034: wallStudSize === '2x4' }"
          @click="wallStudSize = '2x4'"
        >
          <h3>Use 2x4 Wall Studs</h3>
          <p>
            <button @click="showMoreAbout2x4Studs = !showMoreAbout2x4Studs">
              more about 2x4 studs
            </button>
          </p>
        </div>
        <div v-if="showMoreAbout2x4Studs">
          <p>
            2x4 studs are recommended for smaller structures like animal
            housing, and small light duty sheds for storage, gardening tools and
            push type lawnmowers.
          </p>
          Conventional framing practice is to use the same size studs as used
          for floor joists and roof rafters. However, you may choose an
          unconventional approach by mixing sizes.

          <p>
            Using 2x4 studs defaults to conventional 16" on centers. You may
            choose to over-ride that by using more advanced settings later.(this
            feature is currently non-operational)
          </p>
        </div>
        <div class="current_selection" v-if="wallStudSize">
          Selected Wall Stud Size: {{ wallStudSize }}
        </div>

        <div
          class="building-card"
          :class="{ selected_wood034: wallStudSize === '2x6' }"
          @click="wallStudSize = '2x6'"
        >
          <h3>Use 2x6 Wall Studs</h3>
          <p>
            <button @click="showMoreAbout2x6Studs = !showMoreAbout2x6Studs">
              more about 2x6 studs
            </button>
          </p>
          <div v-if="showMoreAbout2x6Studs">
            <p>
              2x6 studs are recommended for larger structures like equipment
              sheds and small buildings.
            </p>
            <p>
              Conventional framing practice is to use the same size studs as
              used for floor joists and roof rafters. However, you may choose an
              unconventional approach by mixing sizes.
            </p>
            <p>
              Using 2x6 wall studs defaults to conventional 24" on centers. You
              may choose to over-ride that by using more advanced settings
              later.(this feature is currently disabled)
            </p>
          </div>
        </div>
        <h2>Roof Peak Direction</h2>

        <p>
          <button @click="showMoreAboutRoofPeak = !showMoreAboutRoofPeak">
            more about roof peaks and gables
          </button>
        </p>
        <div v-if="showMoreAboutRoofPeak">
          <p>
            <strong>Q: What is a roof peak?</strong><br />
            A: A roof peak is the highest point of a roof, typically found at
            the center of a gable roof.
          </p>

          <p>
            <strong>Q: What is a gable?</strong><br />
            A: A gable is the triangular section of a wall between the edges of
            a sloping roof.
          </p>
        </div>

        <div
          class="building-card roof_option"
          @click="roofPeakDirection = 'width'"
        >
          <img
            :src="roofPeakWidth"
            alt="Roof peak runs width"
            class="roof_direction_image"
          />
          <h3>Peak Runs Width</h3>
          <p>Gable ends on the left and right walls.</p>
        </div>

        <p class="current_selection" v-if="roofPeakDirection">
          Selected Roof Direction: {{ roofPeakDirection }}
        </p>

        <div
          class="building-card roof_option"
          @click="roofPeakDirection = 'depth'"
        >
          <img
            :src="roofPeakDepth"
            alt="Roof peak runs depth"
            class="roof_direction_image"
          />
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
              @click="
                showMoreAbout2x4RoofRafters = !showMoreAbout2x4RoofRafters
              "
            >
              More about 2x4 roof rafters
            </button>
          </p>
          <div v-if="showMoreAbout2x4RoofRafters">
            <p>
              2x4 roof rafters are typically used in smaller structures and are
              less expensive than larger rafters.
            </p>
            <p>
              2x4 roof rafters are recommended for smaller structures like
              animal housing, and small light duty sheds for storage, gardening
              tools and push type lawnmowers.
            </p>

            <p>
              Conventional framing practice is to use the same size roof rafters
              as used for floor joists and wall studs. However, you may choose
              an unconventional approach by mixing sizes.
            </p>
            <p>
              Using 2x4 roof rafters defaults to conventional 16" on centers.
              You may choose to over-ride that by using more advanced settings
              later.(this feature is currently disabled)
            </p>
          </div>
        </div>

        <p v-if="roofRafterSize">
          Selected Roof Rafter Size: {{ roofRafterSize }}
        </p>

        <div class="building-card" @click="roofRafterSize = '2x6'">
          <h3>Use 2x6 Roof Rafters</h3>
          <p>
            <button
              @click="
                showMoreAbout2x6RoofRafters = !showMoreAbout2x6RoofRafters
              "
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
              Compared to 2x4 rafters, 2x6 rafters provide greater strength and
              are better suited for longer spans and heavier roof loads.
            </p>

            <p>
              Using 2x6 roof rafters defaults to conventional 24" on-center
              spacing. This can be customized later using advanced settings.
              (This feature is currently disabled.)
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
            A: Roof pitch is the slope of the roof, typically expressed as a
            ratio of rise over run.
          </p>
          <p>
            <strong> Q: What is rise?</strong><br />
            A: Rise is the vertical distance the roof rises over a horizontal
            distance (run).
          </p>
          <p>
            The smaller the ratio, the flatter the roof. A 3:12 ratio is 15
            degrees, a 6:12 ratio is 26.57 degrees, and a 12:12 ratio is 45
            degrees.
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
            siding, doors, windows and walls and can provide additional shade
            and weather protection.
          </p>
        </div>
        <div class="building-card">
          <label class="dimension-label">Eaves Over Hang: </label>
          <input
            type="number"
            v-model="eavesOverHang"
            min="2"
            max="20"
            step="2"
          />
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
            We are currently working on gable ends overhangs and hope to have
            that option available soon.
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
    </div>
  </div>
</template>

<style scoped>
.sticky-tape {
  position: sticky;
  top: 0;
  z-index: 500;
  background: white;
  padding-bottom: 20px;
}
.tape_measure_image {
  display: block;
  width: 100%;
  max-width: 1200px;
  margin: 10px auto;
}

.my-special-heading,
.change-building-link {
  color: #5e3d16; /* darker wood color */
  text-align: center;

  position: relative;
  z-index: 600;

  background: rgba(255, 255, 255, 0.89);
  padding: 4px 12px;
  border-radius: 6px;

  width: fit-content;
  margin: 100px auto 0 auto;
}

.building-card {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 16px;
  margin-top: 12px;
}

.selected-building-banner {
  position: sticky;
  top: 20px;
  z-index: 1000;
  background: transparent;
  border: none;
  width: 100%;
  pointer-events: none;
}

.change-building-link,
.summary-card {
  pointer-events: auto;
}

.configurator-layout {
  display: grid;
  grid-template-columns: minmax(280px, 38%) minmax(0, 62%);
  gap: 20px;
  border: 5px solid red;
}

.page3-sidebar {
  min-width: 0;
  border: 5px solid rgb(60, 255, 0);
  padding: 10px;
  align-self: start;
}

.page3-sticky-panel {
  position: sticky;
  top: 20px;
}

.page3-tape-image {
  display: block;
  width: 100%;
  max-width: 420px;
  margin: 0 auto 16px;
}

.options-column {
  min-width: 0;
  border: 5px solid rgb(60, 255, 0);
  padding: 10px;
}

.main-column {
  min-width: 0;
  padding: 10px;
  border: 5px solid blue;
}

.summary-column {
  width: 225px;
  border: 5px solid rgb(60, 255, 0);
  padding: 10px;
}

.summary-card {
  background: transparent;
  border-radius: 8px;
  padding: 30px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.15);
  width: 100%;
  box-sizing: border-box;
}

.summary-row {
  display: flex;
  justify-content: space-between;
}

.reference-card {
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

.dimensions_reference_image {
  width: 100%;
  max-width: 600px;
  display: block;
  margin-right: auto;
}

.dimension-legend {
  align-self: flex-start;
  margin-bottom: 40px;
}

.legend-width {
  border-left: 8px solid red;
  padding-left: 10px;
  font-size: 1.3rem;
  font-weight: bold;
  margin-bottom: 12px;
}

.legend-depth {
  border-left: 8px solid rgb(93, 245, 93);
  padding-left: 10px;
  font-size: 1.3rem;
  font-weight: bold;
  margin-bottom: 12px;
}

.legend-height {
  border-left: 8px solid blue;
  padding-left: 10px;
  font-size: 1.3rem;
  font-weight: bold;
  margin-bottom: 12px;
}

.warning_box {
  background-color: #fff3cd;
  border: 1px solid #ffc107;
  border-radius: 8px;
  padding: 12px;
  margin: 12px 0;
  color: #856404;
  font-weight: bold;
}

.skids_reference_image {
  width: 100%;
  max-width: 400px;
  display: block;
  margin-right: auto;
}

.current_selection {
  font-size: 1.2rem;
  text-align: center;
  color: #d8b36a;
  text-shadow: 1px 1px 1px black;
  margin: 8px 0;
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

.roof_option {
  display: flex;
  align-items: center;
  gap: 15px;
}

.roof_direction_image {
  width: 120px;
  flex-shrink: 0;
}

@media (max-width: 800px) {
  .configurator-layout {
    grid-template-columns: 1fr;
  }

  .page3-sticky-panel {
    position: static;
  }

  .page3-tape-image {
    max-width: 100%;
  }
}
</style>
