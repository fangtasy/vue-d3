// Import Three.js and any necessary plugins
import * as THREE from 'three';

// Define some constants for the arm's dimensions
const ARM_HEIGHT = 10;
const ARM_WIDTH = 3;
const ARM_DEPTH = 3;
const JOINT_RADIUS = 2;

// Create a Three.js scene, camera, and renderer
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// Create the arm's base
const baseGeometry = new THREE.BoxGeometry(ARM_WIDTH * 2, ARM_HEIGHT / 2, ARM_DEPTH * 2);
const baseMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 });
const base = new THREE.Mesh(baseGeometry, baseMaterial);
base.position.y = ARM_HEIGHT / 4;
scene.add(base);

// Create the arm's first joint
const joint1Geometry = new THREE.SphereGeometry(JOINT_RADIUS);
const joint1Material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
const joint1 = new THREE.Mesh(joint1Geometry, joint1Material);
joint1.position.y = ARM_HEIGHT / 2;
scene.add(joint1);

// Create the arm's first segment
const segment1Geometry = new THREE.BoxGeometry(ARM_WIDTH, ARM_HEIGHT / 2, ARM_DEPTH);
const segment1Material = new THREE.MeshBasicMaterial({ color: 0x0000ff });
const segment1 = new THREE.Mesh(segment1Geometry, segment1Material);
segment1.position.y = ARM_HEIGHT / 4;
segment1.position.z = ARM_DEPTH / 2;
joint1.add(segment1);

// Create the arm's second joint
const joint2Geometry = new THREE.SphereGeometry(JOINT_RADIUS);
const joint2Material = new THREE.MeshBasicMaterial({ color: 0xffff00 });
const joint2 = new THREE.Mesh(joint2Geometry, joint2Material);
joint2.position.y = ARM_HEIGHT / 2;
segment1.add(joint2);

// Create the arm's second segment
const segment2Geometry = new THREE.BoxGeometry(ARM_WIDTH, ARM_HEIGHT / 2, ARM_DEPTH);
const segment2Material = new THREE.MeshBasicMaterial({ color: 0xff00ff });
const segment2 = new THREE.Mesh(segment2Geometry, segment2Material);
segment2.position.y = ARM_HEIGHT / 4;
segment2.position.z = ARM_DEPTH / 2;
joint2.add(segment2);

// Define some variables for animating the arm
let stretch = true;
let stretchAmount = 0;

// Define a function to animate the arm
function animate() {
  requestAnimationFrame(animate);

  // If we're stretching, increase the stretch amount; otherwise, decrease it
  if (stretch) {
    stretchAmount += 0.1;
    if (stretchAmount >= 1) {
      stretch = false;
    }
  } else {
    stretchAmount -= 0.1;
    if (stretchAmount <= 0) {
      stretch = true;
    }
  }

  // Animate the arm based on the stretch amount
  const angle1 = stretchAmount * Math.PI / 2;
  const angle2 = stretchAmount * Math.PI / 4;
  joint1.rotation.z = angle1;
  segment1.rotation.x = -angle1;
  joint2.rotation.z = angle2;
  segment2.rotation.x = -angle2
