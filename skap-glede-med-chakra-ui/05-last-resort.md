```
import { Box, Heading, Image, SimpleGrid, Stack, Text } from '@chakra-ui/core';
import React from 'react';

const kort = [
  {
    id: 1,
    bilde: 'https://picsum.photos/300/200?id=1',
    tittel: 'Enkelt',
    tekst: 'Chakra UI gjør det enkelt å lage kule ting fort!',
  },
  {
    id: 2,
    bilde: 'https://picsum.photos/300/200?id=2',
    tittel: 'Fleksibelt',
    tekst: 'Du kan gjøre hva du vil, uten at Chakra UI kommer i veien',
  },
  {
    id: 3,
    bilde: 'https://picsum.photos/300/200?id=3',
    tittel: 'Blazing fast!',
    tekst: 'Alle biblioteker med respekt for seg selv sier de er blazing fast.',
  },
];

export const App = () => {
  return (
    <Stack spacing={3} as="main">
      <Heading>La oss lage noen kort!</Heading>
      <SimpleGrid columns={[1, 2, 3]} gap={3} as="ul" listStyleType="none">
        {kort.map((etKort) => (
          <Box
            as="li"
            key={etKort.id}
            bgColor="gray.300"
            borderRadius="lg"
            overflow="hidden"
            boxShadow="md"
            transition="all .1s ease-out"
            _hover={{
              boxShadow: 'lg',
              transform: 'scale(1.05)',
            }}
          >
            <Image
              referrerPolicy=""
              src={etKort.bilde}
              alt="Et helt tilfeldig bilde"
              width="100%"
            />
            <Box padding={3}>
              <Heading as="h3" fontSize="xl">
                {etKort.tittel}
              </Heading>
              <Text>{etKort.tekst}</Text>
            </Box>
          </Box>
        ))}
      </SimpleGrid>
    </Stack>
  );
};
```
