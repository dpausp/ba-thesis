.. raw:: latex

    \appendix


**********************
Verwendete Metamodelle
**********************

Editor-Metamodell
=================

.. _anhang-scalamapping:

Programming-Language-Mapping
----------------------------

.. code-block:: java

	level D3 {
		package base {
			concept ScalaMapping {
				1..1 string scalaType;
				0..1 string typeConverter;
			}
		}
	}

.. _anhang-ebl:

Editor-Base-Level
-----------------

.. code-block:: java

	level D2 instanceOf EMM.D3 {
		package types {
			import EMM.D3.base.ScalaMapping;
			ScalaMapping Dimension {
				1..1 real x;
				1..1 real y;
				1..1 real z;
				scalaType = "newsimplex3d.math.float.Vec3";
				typeConverter = "mmpe.model.lmm2scala.Simplex3DVectorConverter";
			}

			ScalaMapping Position {
				1..1 real x;
				1..1 real y;
				1..1 real z;
				scalaType = "newsimplex3d.math.float.Vec3";
				typeConverter = "mmpe.model.lmm2scala.Simplex3DVectorConverter";
			}

			ScalaMapping Rotation {
				1..1 real x0;
				1..1 real x1;
				1..1 real x2;
				1..1 real x3;
				scalaType = "newsimplex3d.math.float.Quat4";
				typeConverter = "mmpe.model.lmm2scala.Simplex3DVectorConverter";
			}

			ScalaMapping Color {
				1..1 real r;
				1..1 real g;
				1..1 real b;
				1..1 real a;
				scalaType = "java.awt.Color";
				typeConverter = "mmpe.model.lmm2scala.AWTTypeConverter";
			}

			ScalaMapping Font {
				0..1 string face;
				0..1 integer size;
				0..1 string style;
				scalaType = "java.awt.Font";
				typeConverter = "mmpe.model.lmm2scala.AWTTypeConverter";
			}

			abstract ScalaMapping PhysicsSettings {
				1..1 real mass;
				1..1 real restitution;
				typeConverter = "mmpe.model.lmm2scala.PhysicsSettingsConverter";
			}

			concept PhysBox extends PhysicsSettings {
				1..1 concept Dimension halfExtends;
				scalaType = "siris.components.physics.PhysBox";
			}

			concept PhysSphere extends PhysicsSettings {
				1..1 real radius;
				scalaType = "siris.components.physics.PhysSphere";
			}
		}
		package figures {
			import EMM.D3.base.ScalaMapping;
			import EMM.D2.types.*;
			abstract ScalaMapping EditorElement {
				1..1 pointer modelElementFQN;
				string toolingIcon = "(none)";
				1..1 boolean interactionAllowed = true;
				0..1 real highlightFactor = 1.3;
			}

			abstract ScalaMapping SceneryObject {
				1..1 string toolingName;
				1..1 string toolingIcon;
				1..1 concept Position pos;
				1..1 concept Dimension dim;
				1..1 concept Rotation rotation;
				0..1 concept PhysicsSettings physics;
			}

			abstract concept Node extends EditorElement {
				1..1 string toolingAttrib;
				1..1 string toolingTitle;
				1..1 concept Position pos;
				1..1 concept Dimension dim;
				1..1 concept Rotation rotation;
			}

			abstract concept Edge extends EditorElement {
				1..1 real thickness;
				1..1 string inboundAttrib;
				1..1 string outboundAttrib;
				1..1 string toolingName;
				1..1 concept Node startNode;
				1..1 concept Node endNode;
			}

			concept ColoredLine extends Edge {
				1..1 concept Color color;
				1..1 concept Color specularColor;
				scalaType = "mmpe.model.figures.ColoredLine";
			}

			concept TexturedLine extends Edge {
				1..1 string texture;
				1..1 concept Color specularColor;
				1..1 concept Color backgroundColor;
				scalaType = "mmpe.model.figures.TexturedLine";
			}

			abstract concept TextLabelNode extends Node {
				1..1 string displayAttrib;
				1..1 concept Font font;
				1..1 concept Color fontColor;
				1..1 concept Color backgroundColor;
			}

			abstract concept TexturedNode extends Node {
				1..1 string texture;
				1..1 concept Color backgroundColor;
			}

			concept TextDiamond extends TextLabelNode {
				scalaType = "mmpe.model.figures.TextDiamond";
			}

			concept RoundedTextBox extends TextLabelNode {
				scalaType = "mmpe.model.figures.RoundedTextBox";
			}

			concept TextBox extends TextLabelNode {
				scalaType = "mmpe.model.figures.TextBox";
			}

			concept TextureBox extends TexturedNode {
				scalaType = "mmpe.model.figures.TextureBox";
			}

			concept TextureDiamond extends TexturedNode {
				scalaType = "mmpe.model.figures.TextureDiamond";
			}

			concept ColladaSceneryObject extends SceneryObject {
				1..1 string sceneryModel;
				scalaType = "mmpe.collada";
			}
		}
	}


.. _anhang-edl:

Editor-Definition-Level (Auszug)
--------------------------------

.. code-block:: java

    Level D1 instanceOf EMM.D2 {
        package nodeFigures {
			concept ProcessNode instanceOf TextBox {
				modelElementFQN = pointer PM.M2.processLanguage.Process;
				displayAttrib = "function";
				toolingAttrib = "function";
				toolingTitle = "Process";
				font = DefaultFont;
				fontColor = Black;
				backgroundColor = LightYellow;
				dim = UnitDimension;
				pos = DefaultPosition;
				rotation = NullRotation;
			}

            concept AndConnectorNode instanceOf TextDiamond {
				texture = "tex/model_textures/and_symbol.png";
				modelElementFQN = pointer PM.M2.processLanguage.AndConnector;
				toolingAttrib = "name";
				toolingTitle = "AND";
				backgroundColor = Orange;
				dim = UnitDimension;
				pos = DefaultPosition;
				rotation = DiamondRot;
			}
        }
        package connectionFigures {
			concept ControlFlowEdge instanceOf TexturedLine {
				toolingName = "Control Flow";
				outboundAttrib = "outboundControlFlows";
				inboundAttrib = "inboundControlFlows";
				modelElementFQN = pointer PM.M2.processLanguage.ControlFlow;
				texture = "tex/model_textures/triangle_half_cyan.png";
				specularColor = White;
				backgroundColor = Red;
				thickness = 0.1;
				highlightFactor = 1.7;
			}

            concept NodeDataEdge instanceOf ColoredLine {
				toolingName = "Functional-Data Assoc";
				outboundAttrib = "outboundNodeDataConnection";
				inboundAttrib = "inboundNodeDataConnection";
				modelElementFQN = pointer PM.M2.processLanguage.NodeDataConnection;
				color = Blue;
				specularColor = White;
				thickness = 0.03;
				highlightFactor = 2.0;
			}
        }
    }

.. _anhang_pmm:

Prozess-Metamodell
==================

.. code-block:: java

    model PMM {
        uri "model:/www.ai4.uni-bayreuth.de/ipm3d/pmm";
        level M2 {
            package processLanguage {
                abstract concept Perspective {
                }

                abstract concept Connection {
                }

                abstract concept DataPerspective extends Perspective {
                }
                
                abstract concept FunctionalPerspective extends Perspective {
                }

                concept ControlFlow extends Perspective {
                    string tag = "(noTag)";
                }

                concept DataFlow extends DataPerspective {
                }

                concept NodeDataConnection extends Connection {
                }

                concept NodeControlFlowLabelConnection extends Connection {
                }

                concept ProcessOrgConnection extends Connection {
                }
                
                concept Node extends FunctionalPerspective {
                    0..* concept ControlFlow inboundControlFlows;
                    0..* concept ControlFlow outboundControlFlows;
                    0..* concept NodeDataConnection outboundNodeDataConnection;
                    0..* concept NodeControlFlowLabelConnection outboundNodeControlFlowLabelConnection;
                }

                concept DataItem extends DataPerspective {
                    1..1 string name;
                    0..* concept DataFlow inboundDataFlows;
                    0..* concept DataFlow outboundDataFlows;
                    0..* concept NodeDataConnection inboundNodeDataConnection;
                }
                
                concept ControlFlowLabel {
                    0..* concept NodeControlFlowLabelConnection inboundNodeControlFlowLabelConnection;
                    1..1 string tag;
                }
                
                concept DataContainer extends Node extends DataPerspective {
                    string name;
                }

                concept Process extends Node {
                    string function;
                    string shortFunction;
                    real duration;
                    integer difficulty;
                    boolean isComposite;
                }

                abstract concept FlowElement extends Node {
                    string name;
                }

                concept StartStopInterface extends FlowElement {
                }

                concept AndConnector extends FlowElement {
                }

                concept OrConnector extends FlowElement {
                }

                concept Decision extends FlowElement {
                }
            }
        }
    }
