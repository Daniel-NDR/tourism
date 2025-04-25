
// pages/destinations.tsx

import Link from "next/link";
import { MapPin, Star } from "lucide-react";
import { Card, CardHeader, CardTitle, CardDescription, CardContent, CardFooter } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { OptimizedImage } from "@/components/optimized-image";
import { GetServerSideProps } from "next";

interface Destination {
  id: number;
  name: string;
  slug: string;
  location: string;
  description: string;
  image: string;
  rating: number;
}

interface Props {
  destinations: Destination[];
}

export default function DestinationsPage({ destinations }: Props) {
  return (
    <div className="container py-12 md:py-24">
      <h1 className="text-4xl font-bold mb-8 text-center">Popular European Cities</h1>
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {destinations.map((destination) => (
          <Card key={destination.id} className="overflow-hidden">
            <div className="relative h-48">
              <OptimizedImage
                src={destination.image}
                alt={destination.name}
                fill
                sizes="(max-width: 768px) 100vw, 33vw"
              />
            </div>
            <CardHeader>
              <div className="flex items-center justify-between">
                <CardTitle>{destination.name}</CardTitle>
                <div className="flex items-center">
                  <Star className="h-4 w-4 fill-primary text-primary" />
                  <span className="ml-1 text-sm">{destination.rating}</span>
                </div>
              </div>
              <CardDescription className="flex items-center">
                <MapPin className="h-4 w-4 mr-1" />
                {destination.location}
              </CardDescription>
            </CardHeader>
            <CardContent>
              <p className="line-clamp-2">{destination.description}</p>
            </CardContent>
            <CardFooter>
              <Button asChild>
                <Link href={`/destinations/${destination.slug}`}>View Details</Link>
              </Button>
            </CardFooter>
          </Card>
        ))}
      </div>
    </div>
  );
}

export const getServerSideProps: GetServerSideProps = async () => {
  const res = await fetch("http://localhost:3000/api/destinations");
  const destinations = await res.json();
  return {
    props: {
      destinations,
    },
  };
};
