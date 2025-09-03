<img width="1920" height="1008" alt="image" src="https://github.com/user-attachments/assets/1e0dbf81-290e-46f7-a2e0-682ae5e77429" />
import React, { ComponentType } from "react";
import {
	MappedComponentProperties,
	withMappable,
} from "@adobe/aem-react-editable-components";
import { ImageText } from "../../../atomic/organisms/ImageText/ImageText";
import {
	ButtonContainer,
	ContentContainer,
	ImageContainer,
} from "../../../atomic/organisms/ImageText/ImageText.style";
import { Card, CardProps } from "../../../atomic/molecules/Card/Card";
import {
	AEMButtonProps,
	Button,
	mapAEMButtonToAtomicProps,
} from "../../../atomic/atoms/button/Button";
import { TeaserModel } from "../../Content/teaser/Teaser";
import { BUTTON, IMAGE, TEASER } from "../../constants/resourceTypes";
import { PlaceholderImage } from "../../../atomic/atoms/placeholderimage/PlaceholderImage";
import { ImageProps } from "../../Content/image/CustomImage";
import {
	GenericMappedContainer,
	ComponentTypeMap,
} from "../../Structure/Container/GenericMappedContainer";

interface ImageTextCompositeProps extends MappedComponentProperties {
	cqItems?: {
		image?: ImageProps;
		container?: any;
		buttonContainer?: any;
	};
	appliedCssClassNames?: string;
}

const mapTeaserToCardProps = (teaser: TeaserModel): CardProps => {
	return {
		title: teaser.title || "",
		description: teaser.description || "",
		link: teaser.actions?.[0]?.url || "",
		linkLabel: teaser.actions?.[0]?.title || "",
		type: "secondary",
		horizontal: true,
	};
};

const ImageTextTeaser = withMappable(
	(props: TeaserModel & MappedComponentProperties) => (
		<Card {...mapTeaserToCardProps(props)} />
	),
	{ emptyLabel: "Teaser", resourceType: TEASER, isEmpty: () => false }
);

const ImageTextButton = withMappable(
	(props: AEMButtonProps & MappedComponentProperties) => {
		const buttonProps = mapAEMButtonToAtomicProps(props);
		return <Button {...buttonProps} />;
	},
	{
		emptyLabel: "Button",
		resourceType: BUTTON,
		isEmpty: (props) => !props.text,
	}
);

const AEMPlaceholderImage = withMappable(
	(props: ImageProps & MappedComponentProperties) => (
		<PlaceholderImage
			{...props}
			imageUrl={props.src}
			altText={props.alt}
			showBackground={false}
		/>
	),
	{
		emptyLabel: "Placeholder Image",
		resourceType: IMAGE,
		isEmpty: (props) => !props.src,
	}
);


const imageTextComponentMap: ComponentTypeMap = {
	[TEASER]: ImageTextTeaser,
	[BUTTON]: ImageTextButton,
};

const ImageTextComposite = (props: ImageTextCompositeProps) => {
	const { cqPath, cqItems, appliedCssClassNames } = props;

	return (
		<ImageText className={appliedCssClassNames || "image-left"}>
			<ImageContainer>
				{cqItems?.image && (
					<AEMPlaceholderImage {...cqItems.image} cqPath={`${cqPath}/image`} />
				)}
			</ImageContainer>
			<ContentContainer>
				{cqItems?.container && (
					<GenericMappedContainer
						{...cqItems.container}
						cqPath={`${cqPath}/container`}
						componentMap={imageTextComponentMap}
					/>
				)}
				{cqItems?.buttonContainer && (
					<ButtonContainer>
						<GenericMappedContainer
							{...cqItems.buttonContainer}
							cqPath={`${cqPath}/buttonContainer`}
							componentMap={imageTextComponentMap}
						/>
					</ButtonContainer>
				)}
			</ContentContainer>
		</ImageText>
	);
};

export default ImageTextComposite as ComponentType<MappedComponentProperties>;


hoje ele tem que trazer se for imagem ele faz como esta acima, se for video ele deve trazer o meu VideoContainer com o <Embed/>



<img width="1920" height="1008" alt="image" src="https://github.com/user-attachments/assets/a07145a2-b20c-4cb6-a48e-cfd1c69e0614" />

import styled from "styled-components";
import { typography } from "../../../base/styled/typography.style";
import { devices, sizes } from "../../../base/styled/variables.styled";

export const ImageTextContainer = styled.div`
	display: flex;
	flex-direction: column;
	gap: 16px;
	max-width: ${sizes.desktop}px;
	margin: 0 auto;
	padding: 0 48px;

	@media ${devices.mobile} {
		padding: 0 48px;
		gap: 64px;
	}

	&.image-left {
		@media (min-width: ${sizes.tablet}px) {
			flex-direction: row;
		}
	}

	&.image-right {
		@media (min-width: ${sizes.tablet}px) {
			flex-direction: row-reverse;
		}
	}

	.list-icon-item {
		align-items: flex-start;
	}

	.cmp-teaser__content {
		${typography.body1}
	}
	.cmp-teaser__title-text {
		${typography.subtitle2}
	}

	.card-container {
		margin: 0;
		max-width: 100%;
	}

	.card-container > div {
		width: 100% !important;
	}

	.list-icon-container {
		margin: 0;
	}
`;

export const ImageContainer = styled.div`
	flex: 1;
	display: block;
	border-radius: 12px;
	position: relative;
	overflow: hidden;
	margin: auto 0;

	img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.cmp-image {
		width: 100%;
		height: 100%;
	}

	.cq-Editable-dom {
		min-width: 200px; // makes the placeholder wider
	}
`;



export const VideoContainer = styled.div`
	flex: 1;
	display: block;
	border-radius: 12px;
	position: relative;
	overflow: hidden;
	margin: auto 0;

	.embed__wrapper {
		width: 100%;
		height: 100%;
	}

	iframe {
		width: 100%;
		height: 100%;
		border: 0;
	}

	.cq-Editable-dom {
		min-width: 200px;
	}
`;


export const ContentContainer = styled.div`
	flex: 1;
	display: flex;
	flex-direction: column;
	gap: 24px;

	.aem-container {
		display: flex;
		flex-direction: column;
		gap: 24px;

		> div:has(> div > button) {
			display: inline-block !important;
		}
	}
`;

export const ButtonContainer = styled.div`
	display: flex;
	gap: 24px;

	> div {
		width: 100%;
	}

	@media ${devices.mobile} {
		flex-direction: column;
		width: 100%;
	}

	.new.section.cq-Editable-dom {
		// makes the placeholder wider
		min-width: 200px;
	}

	.aem-container {
		display: flex;
		flex-direction: row;
		gap: 24px;
		flex-wrap: wrap;

		@media ${devices.mobile} {
			flex-direction: column;
			width: 100%;
		}
	}
`;


temos que criar uma apliedcssclassename para o video assim>
.ps-imagetext-video


